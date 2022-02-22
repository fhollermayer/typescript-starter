# Minimal Typescript Boilerplate

If you are in fact here to use this code, go ahead and just clone the repository into an existing directory.

```shell
npx degit fhollermayer/minimal-typescript-boilerplate
```

## Motivation 
As a programmer, coming from a language like C# or Java, you are most probably used to a workflow as simple as *Start favorite IDE*, *New Project* to run whatever your heart desires. With Typescript or even vanilla JavaScript, that is most often not the case. All the options for pre-compilers, unit-testing frameworks, bundlers, and so on keep you from simply executing a file. More than once, I found myself using https://www.typescriptlang.org/play or https://codesandbox.io/ instead of investing 30 minutes in setting up a simple boilerplate. I finally did it!

## Prerequisites

### git
Even in the most basic application, I find myself in need of some version management. In most cases The file history and `Ctrl + Z` is enough, but from time to time, I wish I just would've persisted the last working state: [Add](https://git-scm.com/docs/git-add) everything not excluded via [.gitignore](https://git-scm.com/docs/gitignore) and [commit](https://git-scm.com/docs/git-commit).

```shell
git add . && git commit -m "no message"
```


### [Node Version Manager](https://github.com/nvm-sh/nvm/blob/master/README.md)
If you do or plan to work with Node.js productively, you are sooner or later in need of multiple Node.js versions simultaneously. `nvm` allows you to install all of them.
`nvm` sets up a SYMLINK from `C:\\Program Files\nodejs` to `%userprofile\.nvm\version\node\...`. You can control which version it takes via `nvm install --lts` or `nvm install 16.13.2` and `nvm use 16.13.2`.
_Beware_: Some Anti-Virus utilities block nvm from unpacking the node directory. In this case get the binaries for [nodejs](https://nodejs.org/en/download/) and unzip them directly into the corresponding directory e.g. `%userprofile\.nvm\version\node\16.13.2`


### VSCode

In my opinion, the best choice for JavaScript/TypeScript. It's even available as a web-app at [vscode.dev](https://vscode.dev/)


### Optional: WSL
If you are on Windows, Powershell is ofc the superior console, but the internet is more UNIX - it's easier to go with that. Go to "Programs and Features," install the "Windows Subsystem For Linux," add a "Distribution" using the store, and you are good to go. Or just simply use PowerShell to [install WSL](https://docs.microsoft.com/en-us/windows/wsl/install).



## Steps to reproduce

<details>
<summary>Set up a project</summary>


```shell
git init && npm init
```

Create a `.gitignore`

```plaintext
node_modules
```

Stage everything and commit the current state

```shell
git add . && git commit -m "initializes project"
```

</details>

<details>
<summary>Add TypeScript</summary>


```shell
npm i -D typescript
```
```shell
./node_modules/.bin/tsc --init
```

Create src/index.ts

```typescript
console.log("It's working!");
```

You may check if the typescript compiler is working as expected by compiling and executing. Clean up after that.

```shell
./node_modules/.bin/tsc --build
```
```shell
node src/index.js
```
```shell
./node_modules/.bin/tsc --build --clean
```

To get rid of the manual compiling step; Install ts-node, which does in on the fly.

```shell
npm i -D ts-node
```

Update tsconfig.json to use a base config (utilizing [@tsconfig/bases](https://github.com/tsconfig/bases))

```json
{
    "extends": "ts-node/node16/tsconfig.json",
    "ts-node": {
        "compilerOptions": {}
    },
    "compilerOptions": {
        /* Visit https://aka.ms/tsconfig.json to read more about this file */
    }
}
```

Update package.json by adding a convenient entry point.

```json
{
    // ...
    "scripts": {
        "start": "ts-node src/index.ts"
    }
    // ...
}
```

Admire the convenience by using:

```shell
npm run start
```

```shell
git add . && git commit -m "adds typescript support"
```

---
</details>

<details>
<summary>Unit-Testing</summary>

[`mocha`](https://mochajs.org/) is the test-runner of my choice and [`chai`](https://www.chaijs.com/) a compatible assertion library.

```shell
npm i -D mocha chai
```

Both modules originate from the realm of javascript, so no typing in the module itself. [@types](https://github.com/DefinitelyTyped/DefinitelyTyped) provides corresponding definition files.

```shell
npm i -D @types/mocha @types/chai
```

`mocha` does not search for typescript files out of the box. Create a [`.mocharc.json`](https://mochajs.org/#configuring-mocha-nodejs) and override the necessary properties to fix that.

```json
{
    "extension": ["ts"],
    "spec": "**/*.spec.ts",
    "require": "ts-node/register"
}
```

Being a righteous software developer, you follow the teachings of [TDD](https://en.wikipedia.org/wiki/Test-driven_development), so you first write a test file `src/index.spec.ts`

```typescript
import {main} from "./index";

describe("main", () => {
    it("runs without error", () => {
        main();
    })
})
```

Make `src/index.ts` a module by exporting its functionality and let it fail

```typescript
export function main() {
    throw new Error("Not implemented yet!");
    // console.log("It's working!");
}

main();
```

Verify the test failure

```shell
./node_modules/.bin/mocha
```

Fix the module

Update src/index.ts

```typescript
export function main() {
    console.log("It's working again!");
}
```

Check you implementation

```shell
./node_modules/.bin/mocha
```

Add a `test` script to `package.json`. (Inside the scripts-section `npm` will save you the effort to add a path-prefix to your executables by automatically searching in `./node_modules/.bin`.)

Update package.json
```json
{
    // ...
    "scripts": {
        // ...
        "test": "mocha"
    }
    // ...
}
```

```shell
git add . && git commit -m "adds unit testing"
```
</details>

<details>
<summary>Documentation</summary>
</details>
