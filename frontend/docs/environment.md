# Environment

## Node.js

Always keep your Node.js environment up to date. Strive for keeping your Node.js version in the latest LTS version. **Don't** use versions outside of the LTS release cycle.

> To check the current release cycle of Node.js, check out [nodejs/Release](https://github.com/nodejs/Release).

## Yarn

- Use yarn. If you encounter projects still using npm, convert them to yarn ASAP.
- Installing yarn will generate a `yarn.lock` file. **Always** commit the `yarn.lock` file into your repository.
- Always periodically check your packages for vulnerabilities with the `yarn audit` command.
