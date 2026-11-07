# How do you use a value exported from another file?

## Theory

## ES Modules

Each `.ts` file is a module. Values are shared with `export` and used with `import`:

```typescript
// test-config.ts
export const APP_BASE_URL = 'http://localhost:8081';
export default class LoginPage {}

// login.spec.ts
import { test, expect } from '@playwright/test';  // named imports from a package
import { APP_BASE_URL } from '../test-config';     // named import from a local file
import LoginPage from '../pages/login.page';       // default import
import type { Page } from '@playwright/test';      // type-only import
```

Named exports must be imported with the same name in curly braces. Relative paths start with `./` or `../`.

## Question

`test-config.ts` contains `export const APP_BASE_URL = '...'`. How do you use it in a test file next to it?

## Options

- **A.** `require APP_BASE_URL`
- **B.** `import APP_BASE_URL from 'test-config'`
- **C.** `import { APP_BASE_URL } from './test-config'`
- **D.** It is available everywhere automatically

## Correct Answer

**C**

## Explanation

**Option C** is correct. A **named** export is imported with curly braces and the same name, and a local file needs a relative path starting with `./`.

- **A** is not valid syntax (CommonJS uses `const x = require('...')`).
- **B** is wrong twice: without braces it is a default import, and without `./` Node looks for a package called `test-config`.
- **D** is wrong: module scope is private; only exported values can be imported.
