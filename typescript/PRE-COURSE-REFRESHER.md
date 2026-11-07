# JavaScript & TypeScript Pre-Course Refresher

## Course
- **Title:** JavaScript and TypeScript Essentials for Test Engineers
- **Difficulty:** beginner
- **Estimated Time:** 45 minutes
- **Tags:** javascript, typescript, async, testing

## Description
Refresh the JavaScript and TypeScript you will read and write every day in Playwright tests: variables, functions, objects, arrays, async/await, modules, types and classes.

## JavaScript and TypeScript Essentials for Test Engineers

TypeScript is JavaScript with types. Everything that works in JavaScript works in TypeScript; TypeScript adds type annotations that your editor and the compiler check before the code runs. Playwright tests are usually written in TypeScript, so this refresher covers both: first the JavaScript you will use constantly, then the TypeScript features that make tests safer and easier to maintain.

You do not need to know everything below by heart. Aim to recognise the syntax when you see it in a test, and to know where to look when something does not compile.

### Running the Examples

Node.js 24 can run TypeScript files directly, which is handy for experimenting:

```bash
node example.ts
```

Node removes the type annotations and runs the remaining JavaScript; it does not check the types. Type checking happens in your editor and in tools such as `tsc --noEmit`. Playwright runs `.ts` test files for you with `npx playwright test`.

### Variables: `const` and `let`

Use `const` by default and `let` only when the variable must be reassigned. Avoid `var`; it has confusing scoping rules.

```typescript
const baseUrl = 'http://localhost:8081';
let attempts = 0;
attempts = attempts + 1; // fine, it is a let

// baseUrl = 'other'; // error: cannot reassign a const
```

`const` prevents reassignment of the variable, not changes inside an object or array: `const user = { name: 'Ann' }; user.name = 'Bob';` is allowed.

### Strings and Template Literals

Template literals use backticks and `${...}` to insert values. You will see them in URLs and test names.

```typescript
const productId = 42;
const url = `/products/${productId}`;
console.log(`Opening ${url}`);
```

### Comparisons: `===` Instead of `==`

Always use strict equality `===` and `!==`. Loose equality `==` converts types first and gives surprising results:

```typescript
console.log(0 == '0');  // true  (type conversion)
console.log(0 === '0'); // false (different types)
```

### Functions and Arrow Functions

Both forms are common. Arrow functions are shorter and are used everywhere for callbacks.

```typescript
function fullName(first: string, last: string): string {
  return `${first} ${last}`;
}

const double = (n: number) => n * 2; // implicit return
const greet = (name: string) => {
  return `Hello, ${name}`;           // block body needs return
};
```

Playwright tests themselves are arrow functions passed to `test()`:

```typescript
test('should log in', async ({ page }) => {
  // test body
});
```

### Objects and Destructuring

Objects group related values. Destructuring pulls properties out into variables, which is exactly what `async ({ page }) => ...` does with Playwright fixtures.

```typescript
const credentials = { username: 'admin', password: 'secret' };

const { username, password } = credentials;
console.log(username); // 'admin'

const { username: login } = credentials; // rename while destructuring
```

The spread operator `...` copies properties, which is useful for building test data variants:

```typescript
const validUser = { username: 'client', email: 'client@example.com' };
const userWithoutEmail = { ...validUser, email: '' };
```

### Arrays

The array methods you will use most return new values instead of changing the original array:

```typescript
const prices = [10, 25, 99];

prices.map(price => price * 2);         // [20, 50, 198]
prices.filter(price => price > 20);     // [25, 99]
prices.find(price => price > 20);       // 25 (first match or undefined)
prices.some(price => price > 90);       // true
prices.includes(25);                    // true
prices.length;                          // 3
```

### Asynchronous Code: Promises and `async`/`await`

Browsers and HTTP calls take time, so almost every Playwright call returns a **Promise**: a value that will be available later. Inside an `async` function, `await` pauses until the Promise settles and gives you its result.

```typescript
async function getUserCount(): Promise<number> {
  const response = await fetch('http://localhost:8081/api/v1/users');
  const users = await response.json();
  return users.length;
}
```

The most common bug in test code is a **missing `await`**: the line starts an action, the test moves on immediately, and later steps run against a page that is not ready, or the test ends before the action finishes.

```typescript
await page.getByRole('button', { name: 'Sign in' }).click(); // correct
page.getByRole('button', { name: 'Sign in' }).click();       // bug: not awaited
```

To run independent async operations at the same time, use `Promise.all`:

```typescript
const [products, orders] = await Promise.all([
  request.get('/api/v1/products'),
  request.get('/api/v1/orders'),
]);
```

Errors in async code are handled with `try`/`catch` around `await`. A rejected Promise that is awaited throws like a normal error.

### Modules: `import` and `export`

Each file is a module. You export what other files may use and import it where needed.

```typescript
// test-config.ts
export const APP_BASE_URL = 'http://localhost:8081';

// login.spec.ts
import { test, expect } from '@playwright/test';
import { APP_BASE_URL } from './test-config';
```

### TypeScript: Types and Annotations

Type annotations describe what a value is. TypeScript reports mismatches before the code runs.

```typescript
const timeout: number = 5000;
const name: string = 'admin';
const enabled: boolean = true;
const ids: number[] = [1, 2, 3];
```

You rarely need to annotate everything: TypeScript **infers** types from values. `const timeout = 5000` is already a `number`. Annotate function parameters, public function results and data shapes.

### Interfaces and Type Aliases

Describe the shape of objects such as request bodies (DTOs) with `interface` or `type`:

```typescript
interface LoginDto {
  username: string;
  password: string;
}

type Role = 'ROLE_ADMIN' | 'ROLE_CLIENT'; // union of allowed values

interface User {
  username: string;
  email: string;
  roles: Role[];
  phone?: string; // optional property
}
```

If a test builds a `LoginDto` and forgets `password`, the editor marks the error immediately.

### Generics

Generic types take another type as a parameter. You mostly read them rather than write them: `Promise<number>` is a promise of a number, `Array<User>` (or `User[]`) is an array of users.

```typescript
async function readJson<T>(response: Response): Promise<T> {
  return (await response.json()) as T;
}
```

### Classes and Page Objects

Page objects are classes that wrap locators and actions for one page:

```typescript
import type { Page, Locator } from '@playwright/test';

export class LoginPage {
  readonly usernameInput: Locator;

  constructor(private readonly page: Page) {
    this.usernameInput = page.getByLabel('Username');
  }

  async goto() {
    await this.page.goto('/login');
  }
}
```

`private readonly page: Page` in the constructor declares and assigns a property in one step. `readonly` means it cannot be reassigned later.

This shorthand is called a parameter property. Playwright runs it fine, but plain `node example.ts` does not support it; when experimenting with Node directly, declare the property and assign it in the constructor body instead.

### `null`, `undefined`, Optional Chaining and `??`

Values can be missing. Optional chaining `?.` stops and returns `undefined` instead of throwing, and `??` provides a default only for `null` or `undefined`:

```typescript
const user = { address: undefined as { city: string } | undefined };
const city = user.address?.city;          // undefined, no crash
const port = Number(process.env.PORT ?? 8081);
```

Prefer `??` over `||` for defaults: `0 || 10` is `10`, but `0 ?? 10` is `0`.

### Types Exist Only Before the Code Runs

TypeScript types are removed before execution. They cannot validate data at runtime: if an API returns a different shape than your interface says, the test still runs and fails later. That is why API tests assert on actual response values, not only on types.

### npm, npx and `package.json`

- `package.json` lists dependencies and scripts.
- `npm ci` installs exactly the versions from `package-lock.json` (use it after cloning).
- `npx playwright test` runs a tool installed in the project.
- `npm run <script>` runs a script defined in `package.json`.

### Conclusion

With `const`/`let`, arrow functions, destructuring, array methods, `async`/`await`, modules, interfaces and classes you can read almost any Playwright test. Pay special attention to `await`: most confusing test failures for newcomers come from a missing one. Continue with the [Playwright refresher](../playwright/PRE-COURSE-REFRESHER.md) once these examples feel familiar.
