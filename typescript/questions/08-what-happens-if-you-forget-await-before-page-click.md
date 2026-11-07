# What happens if you forget await before page.click()?

## Theory

## `await` in Tests

`await` pauses the async function until the Promise settles. Without it, the action is only **started** and the next line runs immediately.

```typescript
test('logs in', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Username').fill('admin');
  page.getByRole('button', { name: 'Sign in' }).click();   // missing await!
  await expect(page).toHaveURL('/');                        // may run too early
});
```

A missing `await` leads to flaky tests, actions that never finish before the test ends, and errors reported at strange places. The `@typescript-eslint/no-floating-promises` lint rule catches it.

## Question

What happens if you call `page.getByRole('button').click()` without `await`?

## Options

- **A.** The click is started, but the test continues immediately without waiting for it to finish
- **B.** TypeScript refuses to run the test
- **C.** Playwright waits anyway, so nothing changes
- **D.** The click is skipped

## Correct Answer

**A**

## Explanation

**Option A** is correct. The call returns a Promise and starts the click, but the test does not wait for it. The next lines can run before the click happens, and if the click fails, the error may surface later or not at all.

- **B** is wrong: this is valid TypeScript; only a lint rule such as `no-floating-promises` reports it.
- **C** is wrong: Playwright auto-waits **inside** the click, but your test must still `await` that Promise.
- **D** is wrong: the click is started; it is just not awaited.
