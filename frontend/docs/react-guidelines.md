# Kata Platform Frontend Development Standard - React Guidelines

*Work in progress*

## Overview

This is a living document outlining the React-specific guidelines in the Kata Platform Frontend Development Standard.

## Styleguide

### Naming

#### Use `.jsx` and/or `.tsx` extensions for React components

Any JS/TS files with a JSX syntax should always be marked as such.

> Note that using `.js` for JSX files will also give problems for Emmet autocompletion on VS Code, see [Microsoft/vscode#4962](https://github.com/Microsoft/vscode/issues/4962).

#### File names should match the component name

When writing React components, file names should match the name of the component being exported.

```tsx
// CheckoutButton.tsx
export const CheckoutButton = () => <button>Checkout</button>;
```

#### Use PascalCase when naming React components.

This is according to the JSX specification. Use the PascalCase convention for filenames, too.

#### Keep component/prop names clear and concise.

Use component names that are as clear and standard as possible. Strive for component names no longer than 2-3 words.

#### Use the `onVerb` prop naming structure for event handlers

For internal event handlers, use the `handleVerb` naming structure.

```tsx
<Pagination onSelectPage={this.handleSelectPage}>
  ...
</Pagination>
```

#### Use the `isVerb` prop naming structure for represeting state

```tsx
<DataTable isLoading={this.props.isLoading}>
  ...
</DataTable>
```

#### Use the `<ComponentName>Props` and `<ComponentName>State` naming structure for component props/state

```tsx
// Bad example
interface Props {}

// Good example
interface CheckoutButtonProps {}
```

### Class components

#### Use `public` for all default React lifecycles

All React component lifecycles (including `static` lifecycles like `getDerivedStateFromProps`) **should** be set to `public`.

#### Use `private` when defining internal class properties

Class properties that are only used privately should be marked as such.

### Function components

#### Use `React.FC<Props>` type over `React.SFC<Props>` type for function components

The `React.SFC` type is deprecated as of `@types/react@16.7.0`, since with the upcoming [Hooks](https://reactjs.org/docs/hooks-intro.html), function components can also have state, so calling them "stateless" is a misnomer.

```tsx
// Bad example
const App: React.SFC = ({ children }) => <div>{children}</div>;
```

```tsx
// Good example
const App: React.FC = ({ children }) => <div>{children}</div>;
```

### Types

#### Don't inline prop types

You should always declare the props of a component separately.

```tsx
// Bad example
const App: React.FC<{ message: string }> = ({ message }) => <div>{message}</div>;
```

```tsx
// Good example
interface AppProps {
  message: string
}

const App: React.FC<AppProps> = ({ message }) => <div>{message}</div>;
```
