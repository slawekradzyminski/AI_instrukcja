# What does constructor(private readonly page: Page) do?

## Theory

## Classes and Page Objects

Page objects are classes that group locators and actions for one page:

```typescript
import type { Page, Locator } from '@playwright/test';

export class LoginPage {
  readonly submitButton: Locator;

  constructor(private readonly page: Page) {
    this.submitButton = page.getByTestId('login-submit-button');
  }

  async login(username: string, password: string) {
    await this.page.getByLabel('Username').fill(username);
    await this.page.getByLabel('Password').fill(password);
    await this.submitButton.click();
  }
}

const loginPage = new LoginPage(page);
await loginPage.login('admin', 'secret');
```

- `new` creates an instance and calls the `constructor`.
- `this` refers to the current instance.
- `readonly` properties cannot be reassigned after construction.

## Question

What does `constructor(private readonly page: Page) {}` do in a TypeScript class?

## Options

- **A.** Creates a global variable `page`
- **B.** Requires the caller to pass `page` as a string
- **C.** Makes `page` available only inside the constructor
- **D.** Declares a private, read-only `page` property and assigns the constructor argument to it

## Correct Answer

**D**

## Explanation

**Option D** is correct. A parameter with an access modifier (`private`, `public`, `readonly`) is a **parameter property**: TypeScript declares the property and assigns the argument automatically, so `this.page` works in every method.

- **A** is wrong: the property belongs to each instance, not to the global scope.
- **B** is wrong: the annotation `Page` requires a Playwright `Page` object.
- **C** is wrong: without the modifier the parameter would be local to the constructor; with it, it becomes a property.

Note that Node's built-in TypeScript support (type stripping) does not support parameter properties; Playwright's own TypeScript handling does.
