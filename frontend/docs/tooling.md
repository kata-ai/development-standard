# Tooling

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

### Prettier

The frontend team uses its own specific TSLint rule. Instead of extending `tslint-config-kata`, extend `tslint-config-kata/react`.

```json
{
  "rulesDirectory": ["tslint-plugin-prettier"],
  "extends": ["tslint-config-kata/react", "tslint-config-prettier"],
  "linterOptions": {
    "exclude": ["node_modules/**"]
  },
  "rules": {
    "prettier": true
  }
}
```

The frontend team uses its own `.prettierrc`:

```json
{
  "useTabs": false,
  "printWidth": 120,
  "tabWidth": 2,
  "singleQuote": true,
  "semi": true
}
```

## Tests

Write tests. Please.

Writing tests ensures that:

- We could add new functionality without breaking old ones
- We could refactor parts of the codebase without fearing that stuff would break.

We use [Jest](https://jestjs.io/) as our test runner. To run test coverage within Jest, run `jest --coverage`. New projects must aim for at least 70% test coverage. Don't forget to use code coverage/code quality tracking tools like [codecov](https://codecov.io/) or [CodeClimate](https://codeclimate.com/).

We use [react-testing-library](https://testing-library.com/docs/react-testing-library/intro) as our React testing framework. This library helps us write better tests for our React components by encouraging good test practices.
