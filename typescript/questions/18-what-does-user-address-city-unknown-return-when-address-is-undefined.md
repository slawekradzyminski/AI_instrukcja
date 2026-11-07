# What does user.address?.city ?? 'unknown' return when address is undefined?

## Theory

## Optional Chaining and Nullish Coalescing

- `a?.b` returns `undefined` instead of throwing when `a` is `null` or `undefined`.
- `x ?? y` returns `y` only when `x` is `null` or `undefined`.

```typescript
const user: { address?: { city: string } } = {};
user.address?.city;               // undefined, no TypeError
user.address?.city ?? 'unknown';  // 'unknown'

0 || 10;   // 10  (|| treats 0, '' and false as missing)
0 ?? 10;   // 0   (?? only replaces null and undefined)
```

`??` is the right choice for defaults such as `process.env.PORT ?? '8081'`.

## Question

With `const user = {}` (no `address`), what is `user.address?.city ?? 'unknown'`?

## Options

- **A.** `'unknown'`
- **B.** A `TypeError`
- **C.** `undefined`
- **D.** `null`

## Correct Answer

**A**

## Explanation

**Option A** is correct. `user.address` is `undefined`, so `user.address?.city` short-circuits to `undefined` instead of throwing. Then `?? 'unknown'` replaces `undefined` with `'unknown'`.

- **B** would happen without `?.`: `user.address.city` throws `Cannot read properties of undefined`.
- **C** is the value of `user.address?.city` alone, before `??` is applied.
- **D** is wrong: nothing here produces `null`.
