# Do TypeScript types check API responses at runtime?

## Theory

## Types Are Removed Before Execution

TypeScript checks types while you write and build the code, then **erases** them. The JavaScript that runs has no types:

```typescript
interface User { username: string; email: string }

const response = await request.get('/api/v1/users/me');
const user = (await response.json()) as User;  // a promise to the compiler, not a check
console.log(user.email.toLowerCase());          // crashes at runtime if email is missing
```

`as User` tells the compiler to trust you; it does not validate the data. To verify a response, assert on its actual values:

```typescript
expect(response.status()).toBe(200);
expect(user.email).toContain('@');
```

## Question

An API test declares `const user = (await response.json()) as User`. What does TypeScript guarantee at runtime?

## Options

- **A.** That the response has every property of `User`
- **B.** That the response is valid JSON
- **C.** Nothing: types are removed before the code runs, so the data must be checked with assertions
- **D.** That `response.json()` never throws

## Correct Answer

**C**

## Explanation

**Option C** is correct. Type annotations and `as` casts exist only at compile time. At runtime the code is plain JavaScript, and `user` is whatever the server returned.

- **A** is wrong: if the API changes, the cast still compiles and the test fails later with a confusing error, unless you assert on the fields.
- **B** and **D** are wrong: invalid JSON makes `response.json()` throw at runtime regardless of types.

That is why good API tests assert on status codes and important fields instead of relying on types.
