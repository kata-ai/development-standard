# Kata Platform Development Standard - Frontend

## Overview

This is a living document outlining guidelines for developing frontend projects within Kata.ai, as well as the tools we use. You can read about the styleguide we follow and the tools we use below.

## Starter kits

Generally, for our single-page apps we use [React](https://reactjs.org) with tooling provided by [create-react-app](https://create-react-app.dev/). For React apps with server-side rendering, we use [Next.js](https://nextjs.org/).

- A frontend starter kit with SSR support powered by Next.js is available (**crucible**)

## Guidelines

### TypeScript Styleguide

At Kata.ai, we use [TypeScript](https://www.typescriptlang.org/) across our entire JavaScript stack. TypeScript combines the familiarity of JavaScript with the power of static typing.

In general, using static typing in your JavaScript code [can help prevent about 15%](https://blog.acolyer.org/2017/09/19/to-type-or-not-to-type-quantifying-detectable-bugs-in-javascript/) of the bugs that end up in committed code. Not only static typing, TypeScript also provides various productivity enhancements like advanced statement completion, as well as smart code refactoring.

This document outlines the general styleguide for TypeScript projects within Kata.ai. Our styleguide is mostly derived from the [Airbnb JavaScript styleguide](https://github.com/airbnb/javascript), with changes made to accomodate TypeScript projects.

[Read the TypeScript Styleguide](./docs/typescript-guidelines.md)

### React Styleguide

[React](https://reactjs.org/) is the framework we choose for building frontend projects at Kata.ai. We chose this framework due to its ease of use, as well as extensive community support and library ecosystem. This document outlines React-specific extensions for our TypeScript Styleguide.

[Read the React Styleguide](./docs/react-guidelines.md)

### Tooling

We're committed to the quality of our development stack and tooling. This document contains guides on the tools we use for daily development as a frontend engineer in Kata.ai. This also contains our unified ESLint config that you can use and extend from.

[Read the Tooling docs](./docs/tooling.md)
