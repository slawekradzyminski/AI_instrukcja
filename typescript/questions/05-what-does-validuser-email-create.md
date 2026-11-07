# What does { ...validUser, email: '' } create?

## Theory

## The Spread Operator

`...` expands an object or array into another one. Later properties override earlier ones:

```typescript
const validUser = { username: 'client', email: 'client@example.com' };
const noEmail = { ...validUser, email: '' };
// { username: 'client', email: '' }  validUser is unchanged

const ids = [1, 2];
const more = [...ids, 3];  // [1, 2, 3]
```

It is a handy way to build test data variants from one valid example.

## Question

What does `{ ...validUser, email: '' }` create?

## Options

- **A.** A new object with all properties of `validUser`, but with an empty `email`
- **B.** It changes `validUser.email` to an empty string
- **C.** A syntax error
- **D.** An array of the properties of `validUser`

## Correct Answer

**A**

## Explanation

**Option A** is correct. The spread copies every property of `validUser` into a **new** object, and the following `email: ''` overrides the copied email.

- **B** is wrong: the original object is not modified. That is why spread is safe for test data variants.
- **C** is wrong: this is standard syntax.
- **D** is wrong: object spread inside `{ }` creates an object; array spread uses `[ ]`.

Note that the copy is shallow: nested objects are shared between the original and the copy.
