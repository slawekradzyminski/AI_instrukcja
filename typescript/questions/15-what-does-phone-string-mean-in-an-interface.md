# What does phone?: string mean in an interface?

## Theory

## Optional Properties and Union Types

A `?` after a property name makes it optional. Its type becomes `string | undefined`:

```typescript
interface User {
  username: string;
  phone?: string;              // may be missing
  status: 'active' | 'locked'; // union of allowed literal values
}

const user: User = { username: 'ann', status: 'active' }; // OK without phone
```

Union types (`A | B`) allow one of several types. TypeScript forces you to handle each case before using a value in a way that only fits one of them.

## Question

What does `phone?: string` mean inside an interface?

## Options

- **A.** `phone` must be a non-empty string
- **B.** `phone` can be any type
- **C.** `phone` is optional; it is either a string or `undefined`
- **D.** `phone` is read-only

## Correct Answer

**C**

## Explanation

**Option C** is correct. The `?` marks the property as optional, so objects without `phone` are valid, and its type is `string | undefined`.

- **A** is wrong: TypeScript types do not validate content such as "non-empty".
- **B** is wrong: the type is still `string` when present.
- **D** describes `readonly phone: string`.
