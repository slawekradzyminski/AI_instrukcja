# How do you handle an error thrown by an awaited call?

## Theory

## Errors in Async Code

An awaited Promise that rejects throws an error at the `await` line. Handle it with `try`/`catch`, and use `finally` for cleanup:

```typescript
try {
  await request.delete(`/api/v1/users/${username}`);
} catch (error) {
  console.warn('Cleanup failed', error);
} finally {
  await request.dispose();
}
```

In tests, usually let errors fail the test. Catch them only when you can do something useful, such as best-effort cleanup.

## Question

How do you handle an error from `await request.get(...)` in an `async` function?

## Options

- **A.** You cannot; async errors are always lost
- **B.** Wrap the `await` in `try`/`catch`
- **C.** Add `.catch` to the function declaration
- **D.** Use `if (error)` after the call

## Correct Answer

**B**

## Explanation

**Option B** is correct. When an awaited Promise rejects, the error is thrown at the `await` and can be caught with `try`/`catch` like any synchronous error.

- **A** is wrong: errors are lost only when the Promise is **not** awaited (or not otherwise handled).
- **C** is wrong: `.catch` is a method on a Promise, not on a function declaration.
- **D** is wrong: there is no implicit `error` variable after a call.
