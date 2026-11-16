# abp-steward-upgrade-product

## Purpose

A throwaway demonstration product for the Autonomous Build Platform team-collaboration release (BC-M9-104 UPGRADE and TEAM live demonstrations). It contains
dummy content only and will be archived or deleted after the demonstration. This repository starts public and is made private during the upgrade demonstration.

## Learning objectives

See how a team of teammates and an ABP station request, implement, review and publish changes to a small product through
GitHub issues and pull requests.

## Contents

- `packages/product/greeting.mjs` — the dummy product module.
- `packages/product/test/greeting.test.mjs` — its test.
- `package.json` — the `npm test` script.
- `.github/workflows/ci.yml` — the CI job `test`, the required check on `main`.

## Public API

`greeting(name)` returns the string `"Hello, <name>!"`.

The parameter is documented as a string, but the module never checks it. `name` is interpolated into a JavaScript template
literal, so a non-string name is converted to text exactly as a template literal converts it — the same rule as
`` `${value}` `` anywhere else in JavaScript — and no error is raised:

```js
greeting(42);        // "Hello, 42!"
greeting(undefined); // "Hello, undefined!"
```

So `greeting(42)` returns `"Hello, 42!"`, because the number `42` converts to the text `42`, and `greeting(undefined)` returns
`"Hello, undefined!"`, because `undefined` converts to the literal text `undefined` rather than to an empty string. Callers who
want a different reading for a missing name must convert the value themselves before the call.

## Data or control flow

A caller passes a name to `greeting`, which returns the formatted string. There is no state and no I/O.

## Usage examples

```js
import { greeting } from "./packages/product/greeting.mjs";
console.log(greeting("team")); // Hello, team!
console.log(greeting(""));     // Hello, !
```

An empty name is a normal input, not a mistake: `greeting("")` returns the string `"Hello, !"`, with nothing between the comma-space
and the exclamation mark.

## Testing

Run `npm test` from the repository root. The same command runs in CI as the required `test` check.

## Error handling

The module performs no validation, so an empty name is accepted rather than rejected: `greeting("")` throws nothing and returns
`"Hello, !"`. There is no input that `greeting` refuses. The test fails with an assertion diff if the greeting text changes.

## Assumptions

Node.js 22 or newer. Dummy content only: never add real code, credentials or customer data.

## Extension guide

Add new functions beside `greeting` in `packages/product/`, with a test in `packages/product/test/`. Changes reach `main`
only through a pull request that passes the `test` check.

## Related files

`package.json`, `.github/workflows/ci.yml`, `packages/product/greeting.mjs`, `packages/product/test/greeting.test.mjs`.
