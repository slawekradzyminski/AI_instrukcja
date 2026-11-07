# What type does TypeScript infer for let retries = 3?

## Theory

## Type Inference

TypeScript infers types from values, so you do not need to annotate everything:

```typescript
let retries = 3;               // number
const baseUrl = '/login';      // the literal type '/login' (a const string)
const users = ['ann', 'bob'];  // string[]
const double = (n: number) => n * 2; // return type number
```

Annotate where inference cannot help: function parameters, empty arrays and data shapes coming from outside (API responses).

## Question

Which type does TypeScript infer for `let retries = 3`?

## Options

- **A.** `any`
- **B.** `string`
- **C.** No type until you add an annotation
- **D.** `number`

## Correct Answer

**D**

## Explanation

**Option D** is correct. TypeScript infers `number` from the initial value, and later `retries = 'three'` would be an error.

- **A** is wrong: `any` is used only when TypeScript cannot infer anything (and is discouraged).
- **B** is wrong: `3` is a number literal.
- **C** is wrong: inference gives every variable a type immediately.

With `const retries = 3`, the inferred type is even narrower: the literal type `3`.
