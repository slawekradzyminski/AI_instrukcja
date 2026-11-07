# What is a Promise?

## Theory

## Promises

Many operations take time: loading a page, clicking, calling an API. Instead of blocking, they return a **Promise**: an object representing a value that will be available later. A Promise is:

- **pending** while the work is in progress,
- **fulfilled** with a value when it succeeds,
- **rejected** with an error when it fails.

```typescript
const responsePromise = fetch('http://localhost:8081/login'); // Promise<Response>
const response = await responsePromise;                        // Response
```

Almost every Playwright method that talks to the browser returns a Promise.

## Question

What is a Promise in JavaScript?

## Options

- **A.** A special kind of loop
- **B.** An object representing the eventual result (or error) of an asynchronous operation
- **C.** A TypeScript-only type annotation
- **D.** A guarantee that code runs without errors

## Correct Answer

**B**

## Explanation

**Option B** is correct. A Promise stands for a result that is not available yet. You get the value with `await` (inside an `async` function) or with `.then()`.

- **A** is wrong: Promises are not loops.
- **C** is wrong: Promises are a JavaScript runtime feature. TypeScript only adds the type `Promise<T>` to describe them.
- **D** is wrong: a Promise can be rejected; the name refers to the promise of a future result, not of success.
