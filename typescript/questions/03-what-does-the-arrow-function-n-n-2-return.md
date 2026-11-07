# What does the arrow function (n) => n * 2 return?

## Theory

## Arrow Functions

Arrow functions are a short way to write functions:

```typescript
const double = (n: number) => n * 2;          // expression body: returns n * 2
const doubleBlock = (n: number) => {          // block body
  return n * 2;                               // needs an explicit return
};
const noReturn = (n: number) => { n * 2; };   // returns undefined!
```

They are used everywhere for callbacks, including `test('name', async ({ page }) => { ... })` and `array.map(item => ...)`.

## Question

What does `const f = (n: number) => n * 2; f(5)` return?

## Options

- **A.** `undefined`, because there is no `return` keyword
- **B.** A function
- **C.** An error: arrow functions need curly braces
- **D.** `10`

## Correct Answer

**D**

## Explanation

**Option D** is correct. An arrow function with an **expression body** (no curly braces) returns the expression automatically, so `f(5)` is `10`.

- **A** would be true only for a block body without `return`, such as `(n) => { n * 2; }`.
- **B** is wrong: calling `f(5)` runs the function and returns its result.
- **C** is wrong: curly braces are optional for a single expression.
