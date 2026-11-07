# What does Promise.all do?

## Theory

## Running Async Work in Parallel

`await` one after another runs operations sequentially. When they are independent, `Promise.all` starts them together and waits for all of them:

```typescript
const [productsResponse, ordersResponse] = await Promise.all([
  request.get('/api/v1/products'),
  request.get('/api/v1/orders'),
]);
```

If any Promise rejects, `Promise.all` rejects with that error. A classic Playwright use is waiting for a response while clicking:

```typescript
const [response] = await Promise.all([
  page.waitForResponse('**/api/v1/users/signin'),
  page.getByRole('button', { name: 'Sign in' }).click(),
]);
```

## Question

What does `await Promise.all([a(), b()])` do?

## Options

- **A.** Runs `a()` and waits for it to finish before starting `b()`
- **B.** Returns the result of whichever Promise finishes first
- **C.** Ignores errors from both Promises
- **D.** Waits for both Promises and returns their results in an array, in the original order

## Correct Answer

**D**

## Explanation

**Option D** is correct. Both operations start right away, `Promise.all` waits until all are fulfilled and returns their results in the same order as the input array.

- **A** describes two separate `await`s in sequence.
- **B** describes `Promise.race`.
- **C** is wrong: if any Promise rejects, `Promise.all` rejects too. Use `Promise.allSettled` to collect all results including errors.
