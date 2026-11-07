# What does const { page } = fixtures do?

## Theory

## Object Destructuring

Destructuring copies properties of an object into variables with the same names:

```typescript
const credentials = { username: 'admin', password: 'secret' };
const { username, password } = credentials;
// username === 'admin', password === 'secret'

const { username: login } = credentials;   // rename: login === 'admin'
const { role = 'ROLE_CLIENT' } = credentials as { role?: string }; // default value
```

Playwright uses it in every test signature: `async ({ page, request }) => { ... }` takes the fixtures object and destructures the `page` and `request` fixtures from it.

## Question

What does `const { page } = fixtures` do?

## Options

- **A.** Creates a new empty object called `page`
- **B.** Assigns `fixtures.page` to a new variable named `page`
- **C.** Copies all properties of `fixtures` into `page`
- **D.** Checks whether `fixtures` has a `page` property and returns a boolean

## Correct Answer

**B**

## Explanation

**Option B** is correct. Destructuring reads `fixtures.page` and stores it in a `page` variable. It is equivalent to `const page = fixtures.page;`.

- **A** is wrong: nothing new is created; an existing property is read.
- **C** describes the spread operator: `const copy = { ...fixtures }`.
- **D** is wrong: if the property does not exist, the variable is simply `undefined`.
