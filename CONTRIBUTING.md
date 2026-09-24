# Contributing

## Erasable TypeScript only

This project restricts TypeScript to **erasable syntax only** — TypeScript that
is purely type-level and disappears when types are stripped, leaving valid
JavaScript. TypeScript features with runtime behavior of their own are **not**
allowed:

- `enum` (use a `const` object plus a union type, or a string-literal union)
- `namespace` / `module` blocks with runtime members
- constructor **parameter properties** (`constructor(private x: number)`)
- `import Foo = require(...)` / `import Foo = Bar.Baz` (use ES `import`)

### Why

Erasable syntax lets the code be consumed by any type-stripping toolchain —
including Node's native TypeScript support and the fast build/transpile paths —
without a full TypeScript-aware compiler, and keeps the emitted JavaScript a
faithful, one-to-one image of the source. This mirrors the convention adopted by
the Agoric engineering team (see the discussion linked from the tracking issue).

### How it is enforced

`tsconfig.json` sets [`erasableSyntaxOnly`](https://www.typescriptlang.org/tsconfig/#erasableSyntaxOnly)
(requires TypeScript ≥ 5.8). The type-checker then reports any non-erasable
construct as an error, so the constraint is a **static check**, not a review
convention:

```sh
npm run typecheck   # tsc; fails on non-erasable syntax
```

CI runs this on every push and pull request via
[`.github/workflows/lint.yml`](.github/workflows/lint.yml). `npm run build` runs
the same check before bundling, so a violation fails the build locally too.
