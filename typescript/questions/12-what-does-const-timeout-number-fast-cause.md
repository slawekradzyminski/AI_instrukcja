# What does const timeout: number = 'fast' cause?

## Theory

## Type Annotations

TypeScript adds types after a colon:

```typescript
const timeout: number = 5000;
const name: string = 'admin';
const isAdmin: boolean = false;
const ids: number[] = [1, 2, 3];

function login(username: string, password: string): Promise<void> { /* ... */ }
```

The compiler and your editor check that values match the declared types. Errors are shown as red underlines while you type and by `tsc --noEmit`.

## Question

What happens with `const timeout: number = 'fast';`?

## Options

- **A.** TypeScript reports a type error: a string is not assignable to a number
- **B.** It works; the value is converted to a number
- **C.** It crashes the browser during the test
- **D.** TypeScript changes the type of `timeout` to `string`

## Correct Answer

**A**

## Explanation

**Option A** is correct. The annotation says `timeout` is a `number`, so assigning the string `'fast'` is a compile-time error (`Type 'string' is not assignable to type 'number'`).

- **B** is wrong: TypeScript never converts values; it only checks them.
- **C** is wrong: the error is found before running anything.
- **D** is wrong: the declared type wins; the assignment is rejected.

Catching such mistakes in the editor is the main reason Playwright tests are written in TypeScript.
