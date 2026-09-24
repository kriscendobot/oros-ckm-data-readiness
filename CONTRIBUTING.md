# Contributing

Use only [erasable TypeScript syntax](https://www.typescriptlang.org/tsconfig/#erasableSyntaxOnly) (no `enum`, runtime `namespace`, parameter properties, or `import x = require()`), so the source runs under type-stripping tools such as Node's native TypeScript support.
`npm run typecheck` enforces this locally and in CI.
