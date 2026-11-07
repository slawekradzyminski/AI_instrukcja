# What does Promise<User[]> describe?

## Theory

## Generics

Generic types take another type as a parameter, written in angle brackets:

```typescript
const ids: Array<number> = [1, 2];   // same as number[]
async function getUsers(): Promise<User[]> { /* ... */ }

function first<T>(items: T[]): T | undefined {
  return items[0];
}
first([1, 2]);        // T is number
first(['a', 'b']);    // T is string
```

In Playwright code you mostly read generics: `Promise<void>`, `Locator[]`, `test.extend<{ loginPage: LoginPage }>()`.

## Question

What does the return type `Promise<User[]>` describe?

## Options

- **A.** An array of Promises
- **B.** A Promise that resolves to an array of `User` objects
- **C.** A function that returns either a Promise or users
- **D.** A `User` class that extends `Promise`

## Correct Answer

**B**

## Explanation

**Option B** is correct. `Promise<T>` is a Promise whose value is `T`, and here `T` is `User[]`. After `await`, you get an array of users.

- **A** would be `Promise<User>[]`.
- **C** would be `Promise<User[]> | User[]`.
- **D** is wrong: generics pass types as parameters; they do not create subclasses.
