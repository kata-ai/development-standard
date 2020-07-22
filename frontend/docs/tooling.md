# Tooling

## Overview

We're committed to the quality of our development stack and tooling. This document contains guides on the tools we use for daily development as a frontend engineer in Kata.ai. This also contains our unified ESLint config that you can use and extend from.

## Table of Contents

- [IDE](#ide)
- [TypeScript](#typescript)
- [Linting and Formatting](#linting-and-formatting)
- [Tests](#tests)

---

## IDE

Use [Visual Studio Code](https://code.visualstudio.com). It has first-class support for JavaScript and TypeScript, complete with code-aware statement completion, Emmet integration for JSX elements, as well top-notch debugging environment.

## TypeScript

Below are guides on how we use TypeScript projects.

### Prefer using Babel TS preset on frontend projects

The TypeScript compiler supports most modern ES2015+ JavaScript features up to stage-3 proposals. However, in some cases, chaining TypeScript with Babel, or using the Babel transpiler altogether allows us to use features like hot reloading, additional Babel plugins for e.g. `styled-components`, and a faster compile time.

CRA 2.1 already uses Babel as the default TypeScript transpilation pipeline, so everything's already handled for you.

Note that the Babel compiler **only** removes the types from your code. **It does not do any additional type-checking!** If compiling TS using Babel, make sure to run type-checking separately with `tsc --noEmit`. It's also better to include in your CI process!

### Use `--strict` mode

Make sure to enable strict mode in your tsconfig! Anything less will cause a huge mess in our codebase further down the road.

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

If you do need to disable some rules that the `--strict` flag emits, override it after the `strict` rule in your tsconfig:

```json
{
  "compilerOptions": {
    "strict": true,
    "strictBindCallApply": false
  }
}
```

## Linting and Formatting

### ESLint

Kata.ai provides a [custom ESLint config](https://github.com/kata-ai/eslint-config-kata) to enforce the styleguides outlined in this document. To use it, install `eslint-config-kata`.

```sh
$ yarn add --dev eslint-config-kata
```

Then, extend `eslint-config-kata/react` in your `.eslintrc` file.

```json
{
  "extends": ["eslint-config-kata/react"]
}
```

### Prettier

[Prettier](https://prettier.io/) is a tool to automatically format your code during save. It supports various editors, from VSCode, Atom, Sublime, and even Emacs.

To use this ESLint config in conjunction with Prettier, copy the following `.prettierrc` template:

```json
{
  "printWidth": 120,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "jsxBracketSameLine": false,
  "arrowParens": "avoid",
  "endOfLine": "auto"
}
```

Then install the Prettier eslint config and plugin:

```sh
$ yarn add --dev eslint-plugin-prettier eslint-config-prettier prettier
```

And finally, include them as follows. (**IMPORTANT:** `eslint-config-prettier` MUST be extended after `eslint-config-kata`!)

```json
{
  "extends": ["eslint-config-kata/react", "prettier", "prettier/@typescript-eslint", "plugin:prettier/recommended"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

## Tests

Write tests. Please.

Writing tests ensures that:

- We could add new functionality without breaking old ones
- We could refactor parts of the codebase without fearing that stuff would break.

We use [Jest](https://jestjs.io/) as our test runner. To run test coverage within Jest, run `jest --coverage`. New projects must aim for at least 70% test coverage. Don't forget to use code coverage/code quality tracking tools like [codecov](https://codecov.io/) or [CodeClimate](https://codeclimate.com/).

We use [react-testing-library](https://testing-library.com/docs/react-testing-library/intro) as our React testing framework. This library helps us write better tests for our React components by encouraging good test practices.
