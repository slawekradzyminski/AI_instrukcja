# How does Playwright handle test parallelization by default?

## Theory

## Parallel Test Execution and Performance

Playwright Test is designed from the ground up to run tests in parallel, significantly reducing test suite execution time while maintaining test isolation and reliability.

### Default Parallel Behavior

**Automatic Parallelization**:
```javascript
// Playwright runs tests in parallel by default
// No configuration needed for basic parallel execution

test('test 1', async ({ page }) => { /* runs in worker 1 */ });
test('test 2', async ({ page }) => { /* runs in worker 2 */ });  
test('test 3', async ({ page }) => { /* runs in worker 3 */ });
test('test 4', async ({ page }) => { /* runs in worker 1 again */ });
```

### Controlling Parallelization

**Worker Configuration**:
```javascript
// playwright.config.js
export default defineConfig({
  workers: 4,                    // Use 4 parallel workers
  // or
  workers: process.env.CI ? 2 : 4, // Different settings for CI
  
  fullyParallel: true,           // Run tests within files in parallel
});
```

**Command Line Control**:
```bash
# Specify number of workers
npx playwright test --workers=8

# Disable parallelization  
npx playwright test --workers=1

# Max workers (use all CPU cores)
npx playwright test --workers=100%
```

### Test-Level Parallel Control

**Serial Test Execution**:
```javascript
// Force tests to run sequentially when needed
test.describe.serial('Authentication flow', () => {
  test('login', async ({ page }) => {
    // This runs first
  });
  
  test('access protected page', async ({ page }) => {
    // This runs after login test
  });
});
```

**Limiting Concurrency for Specific Tests**:
```javascript
test.describe('Database tests', () => {
  test.describe.configure({ mode: 'serial' });
  
  test('create record', async ({ page }) => { /* ... */ });
  test('update record', async ({ page }) => { /* ... */ });
  test('delete record', async ({ page }) => { /* ... */ });
});
```

### Performance Optimization

**Efficient Resource Usage**:
```javascript
// Each worker gets its own:
// - Browser instance  
// - Browser contexts (for test isolation)
// - File system access
// - Memory allocation

// Workers reuse browser instances across tests
// Only contexts are recreated for isolation
```

**Test Sharding for CI**:
```bash
# Split tests across multiple machines
# Machine 1:
npx playwright test --shard=1/3

# Machine 2:  
npx playwright test --shard=2/3

# Machine 3:
npx playwright test --shard=3/3
```

### Monitoring Parallel Execution

**Test Reports Show Parallel Info**:
```javascript
// HTML report shows:
// - Total execution time
// - Per-worker utilization  
// - Test distribution across workers
// - Parallel efficiency metrics
```

**Debug Parallel Issues**:
```bash
# Run with detailed output
npx playwright test --reporter=line

# See which tests run on which workers
npx playwright test --reporter=list
```

## Question

How does Playwright handle test parallelization by default?

## Options

- **A.** All tests run sequentially by default
- **B.** Tests only run in parallel on CI servers
- **C.** Tests run in parallel using multiple worker processes
- **D.** Parallelization must be manually configured

## Correct Answer

**C**

## Explanation

## Why Option C is Correct

**Option C** "Tests run in parallel using multiple worker processes" accurately describes Playwright's default parallelization approach.

### Worker-Based Parallel Architecture

Playwright uses a **multi-worker process model** for parallel execution:

```javascript
// Default behavior - no configuration needed
test('homepage loads', async ({ page }) => {
  // Worker 1: Gets this test
});

test('login works', async ({ page }) => {
  // Worker 2: Gets this test  
});

test('dashboard displays', async ({ page }) => {
  // Worker 3: Gets this test
});

test('profile updates', async ({ page }) => {
  // Worker 1: Gets this test (worker reused)
});
```

### How Worker Processes Function

**Automatic Worker Management**:
```javascript
// Playwright automatically:
// 1. Detects available CPU cores
// 2. Creates worker processes (typically 50% of cores)
// 3. Distributes tests across workers
// 4. Each worker runs tests sequentially within itself
// 5. Workers run in parallel with each other
```

**Worker Isolation**:
```javascript
// Each worker process has:
// - Own browser instance
// - Separate memory space  
// - Independent file system access
// - Isolated test execution environment

// This prevents tests from interfering with each other
```

### Default Configuration

```javascript
// playwright.config.js (default behavior)
export default defineConfig({
  workers: process.env.CI ? 1 : undefined, // Local: auto-detect, CI: 1
  // undefined = 50% of CPU cores
  
  fullyParallel: false, // Tests within a file run sequentially
});
```

**Typical Worker Count**:
```bash
# 8-core machine → 4 workers by default
# 4-core machine → 2 workers by default  
# CI environment → 1 worker (for stability)
```

### Analysis of Other Options:

**❌ Option A** "All tests run sequentially by default":
```javascript
// This would be very slow for large test suites
// Playwright's strength is parallel execution
```

**❌ Option B** "Tests only run in parallel on CI servers":
```javascript
// Parallel execution works locally and in CI
// Actually, CI often uses fewer workers for stability
```

**❌ Option D** "Parallelization must be manually configured":
```javascript
// Parallel execution is enabled by default
// Manual configuration is optional for customization:
workers: 8,        // Optional override
fullyParallel: true, // Optional enhancement
```

### 🎯 Key Benefits

**Speed**: Multiple tests execute simultaneously
**Isolation**: Each worker process is independent  
**Efficiency**: Optimal CPU utilization without manual setup
