# What does 0 === '0' evaluate to?

## Theory

## Strict and Loose Equality

JavaScript has two equality operators:

- `===` (strict) compares value **and** type without conversion.
- `==` (loose) converts types before comparing, which leads to surprises.

```typescript
0 == '0';        // true
0 === '0';       // false
null == undefined;  // true
null === undefined; // false
```

In tests always use `===` and `!==`. Playwright assertions such as `expect(status).toBe(200)` also compare strictly.

## Question

What does `0 === '0'` evaluate to?

## Options

- **A.** `true`, because both represent zero
- **B.** It throws a TypeError
- **C.** `false`, because the types differ
- **D.** `undefined`

## Correct Answer

**C**

## Explanation

**Option C** is correct. `===` checks the type first: `0` is a `number`, `'0'` is a `string`, so the result is `false`.

- **A** describes the loose operator `==`, which converts `'0'` to a number first.
- **B** is wrong: comparisons never throw for different types.
- **D** is wrong: comparison operators always return a boolean.

In TypeScript you would also get a compile-time warning, because comparing a `number` with a `string` is almost always a mistake.
