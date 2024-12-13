# pnpm-undeclared-dependencies

This repository is a minimal reproduction of `pnpm` bin wrappers like `node_modules/.bin/jest` allowing to load undeclared dependencies.

## Reproduction

There are two packages in the workspace:

-   `dependency-present` - has a `dependency` on `yaml`
-   `dependency-missing` - does NOT have any `dependency` on `yaml`

`pnpm` should not allow loading undeclared dependencies.

Each package contains two scripts:

-   `test:bin` - runs `jest` through shell wrapper provided by `pnpm` in `node_modules/.bin/jest`
-   `test:node` - runs `jest` through `node` using `node_modules/jest/bin/jest.js`

In order to reproduce the issue, run the following commands:

```sh
pnpm install
pnpm -F dependency-present test:node # control
pnpm -F dependency-present test:bin # control
pnpm -F dependency-missing test:node # fails as expected
pnpm -F dependency-missing test:bin # passes but should fail
```

Running `jest` through `node_modules/jest/bin/jest.js` in `dependency-missing` package fails as expected.

Running `jest` through shell wrapper `node_modules/.bin/jest` in `dependency-missing` package should fail because `yaml` is not declared as a dependency.
