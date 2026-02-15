# How does Playwright ensure test isolation between different tests?

## Theory

## Test Isolation in Playwright

Test isolation is a fundamental principle that ensures each test runs independently without affecting others. Playwright achieves this through browser contexts - isolated environments that prevent tests from interfering with each other.

### What is Test Isolation?

Test isolation means:
- Each test starts with a **clean slate**
- No shared state between tests  
- Tests can run in **any order**
- One failing test doesn't affect others

### How Playwright Achieves Isolation

**Browser Context per Test**:
```javascript
test('login test', async ({ page }) => {
  // Fresh browser context with:
  // - Empty cookies
  // - No localStorage/sessionStorage  
  // - Clean cache
  // - Fresh browser profile
});

test('shopping test', async ({ page }) => {
  // Completely separate context
  // No data from previous test
});
```

### Traditional Approach (Problems)

```javascript
// Shared browser state - causes issues
beforeAll(async () => {
  browser = await chromium.launch();
  page = await browser.newPage(); // Same page for all tests!
});

test('test 1', async () => {
  await page.goto('/login');
  // Leaves cookies and state
});

test('test 2', async () => {
  // May be affected by test 1's state!
});
```

### Playwright's Solution

```javascript
// Each test gets isolated context automatically
test('user can login', async ({ page }) => {
  // Fresh context - no previous state
  await page.goto('/login');
});

test('user can register', async ({ page }) => {
  // Completely separate context
  // No login state from previous test
});
```

### Benefits of Isolation

**Parallel Execution**:
```javascript
// Tests can run simultaneously without conflicts
npx playwright test --workers=4
```

**Debugging Ease**:
```javascript
// Run specific test without setup
npx playwright test --grep "login test"
```

**Reliability**:
```javascript
// No flaky failures due to test order
// Each test is deterministic
```

## Question

How does Playwright ensure test isolation between different tests?

## Options

- **A.** Tests share the same browser instance
- **B.** Tests must be run sequentially
- **C.** Each test runs in a fresh browser context
- **D.** All tests share cookies and local storage

## Correct Answer

**C**

## Explanation

## Why Option C is Correct

**Option C** "Each test runs in a fresh browser context" describes Playwright's isolation mechanism.

### Browser Context Isolation

Playwright automatically creates a **new browser context** for each test:

```javascript
test('first test', async ({ page }) => {
  // Context A: Fresh environment
  await page.goto('/app');
  await page.fill('#user', 'alice');
  // Leaves behind: cookies, localStorage, session
});

test('second test', async ({ page }) => {
  // Context B: Completely separate environment  
  // No cookies, localStorage, or session from Context A
  await page.goto('/app');
  // Starts completely fresh
});
```

### What Browser Context Provides

**Isolated Storage**:
```javascript
// Each context has separate:
// - Cookies
// - localStorage  
// - sessionStorage
// - IndexedDB
// - Cache
```

**Independent Sessions**:
```javascript
test('admin test', async ({ page }) => {
  await page.goto('/login');
  await page.fill('#user', 'admin');
  // Admin session in Context A
});

test('user test', async ({ page }) => {
  await page.goto('/login');  
  await page.fill('#user', 'regular-user');
  // Separate session in Context B
});
```

### Analysis of Other Options:

**❌ Option A** "Tests share the same browser instance":
```javascript
// This would cause state contamination
// Tests would affect each other
```

**❌ Option B** "Tests must be run sequentially":
```javascript
// Playwright supports parallel execution
npx playwright test --workers=4 // ✅ Runs in parallel
```

**❌ Option D** "All tests share cookies and local storage":
```javascript
// This defeats the purpose of isolation
// Would lead to flaky, order-dependent tests
```

### 🎯 Key Benefits

**Parallel Safety**: Tests can run simultaneously without conflicts
**Debugging**: Each test is independent and reproducible  
**Reliability**: No mysterious failures due to shared state
