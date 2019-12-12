# Kata Platform Development Standard - Frontend Guidelines

## Overview

This is a living document outlining guidelines for developing frontend projects and tools within the Kata Platform. You can read about the styleguide we follow and the tools we use below.

The following guides are seen as an extension of the [general guidelines](../general/README.md). All rules outlined there still apply.

## See also

- [React Guidelines](./docs/react-guidelines.md) - for React-specific guidelines.

## Starter kits

Generally, for our single-page apps we use [React](https://reactjs.org) with tooling provided by [create-react-app](https://facebook.github.io/create-react-app/). For React apps with server-side rendering, we use [Next.js](https://nextjs.org/).

- A frontend starter kit with our preconfigured create-react-app setup, complete with our component library, is available here: https://bitbucket.org/yesboss/frontend-starter-internal.
- A frontend starter kit with SSR support powered by Next.js is available here: https://bitbucket.org/yesboss/ssr-starter-internal.

## Tooling

### IDE

Use [Visual Studio Code](https://code.visualstudio.com). It has first-class support for JavaScript and TypeScript, complete with code-aware statement completion, Emmet integration for JSX elements, as well top-notch debugging
environment.

### TypeScript

At Kata.ai, we use [TypeScript](https://www.typescriptlang.org/) across our entire JavaScript stack. TypeScript combines the familiarity of JavaScript with the power of static typing.

In general, using static typing in your JavaScript code [can help prevent about 15%](https://blog.acolyer.org/2017/09/19/to-type-or-not-to-type-quantifying-detectable-bugs-in-javascript/) of the bugs that end up in committed code. Not only static typing, TypeScript also provides various productivity enhancements like advanced statement completion, as well as smart code refactoring.

#### Prefer using Babel TS preset on frontend projects

The TypeScript compiler supports most modern ES2015+ JavaScript features up to stage-3 proposals. However, in some cases, chaining TypeScript with Babel, or using the Babel transpiler altogether allows us to use features like hot reloading, additional Babel plugins for e.g. `styled-components`, and a faster compile time.

CRA 2.1 already uses Babel as the default TypeScript transpilation pipeline, so everything's already handled for you.

Note that the Babel compiler **only** removes the types from your code. **It does not do any additional type-checking!** If compiling TS using Babel, make sure to run type-checking separately with `tsc --noEmit`. It's also better to include in your CI process!

#### Use `--strict` mode

Make sure to enable strict mode in your tsconfig! Anything less will cause a huge mess in our codebase further down the road.

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

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

We use [Jest](https://jestjs.io/) as our test runner. To run test coverage within Jest, run `jest --coverage`. New projects must aim for at least 70% test coverage.

Don't forget your integration tests!

## Styleguide

This styleguide should be seen as an extension of the styleguide defined in the [General Guidelines](./general-guidelines.md). All the rules outlined there still apply.

### Indentation & Line length

### Naming

#### Keep filenames clear and concise

Use filenames that are as clear and standard as possible. Strive for names no longer than 2-3 words, with unabbreviated phrases.

- **Good example:** `findPathOptions.ts`
- **Bad example:** `fpOpts.ts`

### Functions

#### Declare function return type

If declaring a function, you should always specify the return type, even if it is nothing (i.e. `void`.)

```typescript
// Bad example
function healthCheck() {
  return fetch(apiUrl + '/healthCheck');
}

// Good example
function healthCheck(): Promise<HealthCheckResponse> {
  return fetch(apiUrl + '/healthCheck');
}
```

#### Declare function argument type

The type of a function's arguments must be declared, even if it's `any`.

```ts
// Bad example
function add(a, b): number {
  return a + b;
}

// Good example
function add(a: number, b: number): number {
  return a + b
}
```

### Classes

#### Declare access modifiers for each class property

You should always specify whether a class property is `public`, `private`, or `protected`.

```ts
class Button extends React.Component {
  constructor() {}

  // Bad example
  render(): JSX.Element {}

  // Good example
  public componentDidMount(): void {}
}
```

> React component lifecycles are generally classified as `public`, so use that whenever you come across one.

### Interfaces

#### Use PascalCase when declaring interfaces.

```ts
// Bad example
interface checkoutButtonProps {}

// Good example
interface CheckoutButtonProps {}
```

#### Don't use `I` as a prefix when declaring interfaces.

This convention is a leftover from early-day TS when people who mainly do C# started trying out TS, and is generally frowned upon nowadays, especially in frontend TS code.

We avoid prefixing interfaces with `I` specifically in the frontend.

```ts
// Bad example
interface ICheckoutButtonProps {}

// Good example
interface CheckoutButtonProps {}
```

### Enums

#### Use PascalCase when declaring enums.

```ts
// Bad example
enum botStates {}

// Good example
enum BotStates {}
```

#### Avoid `const` enums

Tools like Babel [don't support this syntax](https://babeljs.io/docs/en/babel-plugin-transform-typescript.html#caveats). Use regular enums instead, or just use a plain old JS object.

### Modules

#### Prefer using named exports over default exports

All modules should use named exports where possible. It's easier to refactor our codebase that way.

The only exception to this rule is when a module is being code-split (e.g. a lazy-loaded React component).

#### Prefer using a single entry point

When exporting modules from multiple files within the same folder, collect those exports inside an `index.js` file in the root level of that folder.

```
components/
└── Button/
    ├── index.ts
    ├── Button.tsx
    ├── LinkButton.tsx
    ├── HollowButton.tsx
    └── IconicButton.tsx
```

```ts
// ./components/Button/index.ts

export * from './Button';
export * from './LinkButton';
export * from './HollowButton';
export * from './IconicButton';
```
