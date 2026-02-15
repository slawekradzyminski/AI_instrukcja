# What is the main purpose of fixtures in Playwright?

## Theory

## Playwright Fixtures: Powerful Test Setup and Dependency Injection

Fixtures in Playwright are a powerful dependency injection system that provides reusable test setup and teardown logic. They enable you to create consistent, maintainable test environments while keeping your test code clean and focused.

### Built-in Fixtures

Playwright provides several built-in fixtures automatically:

```javascript
test('example test', async ({ page, context, browser, request }) => {
  // page: A Page instance for browser automation
  // context: BrowserContext for isolation
  // browser: Browser instance  
  // request: APIRequestContext for API calls
});
```

### Creating Custom Fixtures

**Simple Fixture**:
```javascript
import { test as base } from '@playwright/test';

const test = base.extend({
  todoPage: async ({ page }, use) => {
    // Setup: Navigate to todo page
    await page.goto('/todos');
    
    // Provide the fixture to the test
    await use(page);
    
    // Teardown: Cleanup if needed
    console.log('Test completed');
  },
});

test('can add todo', async ({ todoPage }) => {
  await todoPage.fill('#new-todo', 'Learn Playwright');
  await todoPage.press('#new-todo', 'Enter');
  // todoPage is already on /todos URL
});
```

**Advanced Fixture with Data**:
```javascript
const test = base.extend({
  authenticatedUser: async ({ page, request }, use) => {
    // Create user via API
    const userResponse = await request.post('/api/users', {
      data: { email: 'test@example.com', password: 'password123' }
    });
    const user = await userResponse.json();
    
    // Login via UI
    await page.goto('/login');
    await page.fill('#email', user.email);
    await page.fill('#password', 'password123');
    await page.click('#login');
    
    // Provide authenticated page and user data
    await use({ page, user });
    
    // Cleanup: Delete test user
    await request.delete(`/api/users/${user.id}`);
  },
});
```

### Worker-Scoped Fixtures

Some fixtures should be shared across multiple tests:

```javascript
const test = base.extend({
  workerStorageState: [async ({ browser }, use) => {
    // Runs once per worker
    const context = await browser.newContext();
    const page = await context.newPage();
    
    // Login once
    await page.goto('/login');
    await page.fill('#email', 'admin@test.com');
    await page.click('#login');
    
    // Save authentication state
    await context.storageState({ path: 'admin-storage.json' });
    await context.close();
    
    await use('admin-storage.json');
  }, { scope: 'worker' }],
});
```

## Question

What is the main purpose of fixtures in Playwright?

## Options

- **A.** Provide reusable setup and teardown logic for tests
- **B.** Generate test data automatically
- **C.** Run tests in parallel across browsers
- **D.** Create assertions for test validation

## Correct Answer

**A**

## Explanation

## Why Option A is Correct

**Option A** "Provide reusable setup and teardown logic for tests" accurately describes the main purpose of Playwright fixtures.

### Fixtures as Setup/Teardown System

Fixtures provide a **dependency injection system** that handles test preparation and cleanup:

```javascript
const test = base.extend({
  authenticatedPage: async ({ page, request }, use) => {
    // 🔧 SETUP: Prepare test environment
    const user = await request.post('/api/users', {
      data: { email: 'test@example.com', password: 'pass123' }
    });
    
    await page.goto('/login');
    await page.fill('#email', 'test@example.com');
    await page.click('#login');
    
    // ✅ USE: Provide prepared resource to test
    await use(page);
    
    // 🧹 TEARDOWN: Cleanup after test
    await request.delete(`/api/users/${user.id}`);
  },
});

test('user can access dashboard', async ({ authenticatedPage }) => {
  // Test starts with authenticated user already logged in
  await expect(authenticatedPage.locator('.welcome')).toBeVisible();
});
```

### Key Benefits of Fixtures

**Reusability**:
```javascript
// Multiple tests can use the same fixture
test('test 1', async ({ authenticatedPage }) => { /* test logic */ });
test('test 2', async ({ authenticatedPage }) => { /* test logic */ });
test('test 3', async ({ authenticatedPage }) => { /* test logic */ });
```

**Automatic Lifecycle Management**:
```javascript
// Playwright handles setup/teardown automatically
const test = base.extend({
  testData: async ({ request }, use) => {
    const data = await request.post('/api/test-data');
    await use(data);                    // Test runs here
    await request.delete('/api/cleanup'); // Auto cleanup
  },
});
```

**Dependency Injection**:
```javascript
// Tests declare what they need, fixtures provide it
test('needs auth and data', async ({ authenticatedPage, testDatabase }) => {
  // Everything is prepared and ready to use
});
```

### Analysis of Other Options:

**❌ Option B** "Generate test data automatically":
```javascript
// While fixtures CAN generate data, that's not their main purpose
// Test data generation is just one possible use case
```

**❌ Option C** "Run tests in parallel across browsers":
```javascript
// Parallel execution is handled by Playwright's test runner
// Fixtures don't control parallelization
```

**❌ Option D** "Create assertions for test validation":
```javascript
// Assertions are provided by expect() functions
// Fixtures prepare test environment, not validate results
```

### 🎯 Core Concept
Fixtures are **dependency providers** - they set up what tests need and clean up when tests finish.
