# What type of performance testing is most suitable for Playwright?

## Theory

## Performance Testing and Optimization

While Playwright is primarily an end-to-end testing tool, it provides capabilities to measure and optimize web application performance. Understanding performance testing helps identify bottlenecks and ensure optimal user experience.

### Measuring Core Web Vitals

**Largest Contentful Paint (LCP)**:
```javascript
test('measures page loading performance', async ({ page }) => {
  await page.goto('/product-page');
  
  // Measure LCP using PerformanceObserver
  const lcp = await page.evaluate(() => {
    return new Promise(resolve => {
      new PerformanceObserver(list => {
        const entries = list.getEntries();
        const lastEntry = entries[entries.length - 1];
        resolve(lastEntry.startTime);
      }).observe({ type: 'largest-contentful-paint', buffered: true });
    });
  });
  
  // LCP should be under 2.5 seconds for good performance
  expect(lcp).toBeLessThan(2500);
});
```

**First Input Delay (FID) Simulation**:
```javascript
test('measures interaction responsiveness', async ({ page }) => {
  await page.goto('/interactive-app');
  
  const startTime = Date.now();
  await page.click('#heavy-computation-button');
  
  // Wait for visible response
  await expect(page.locator('.result')).toBeVisible();
  const endTime = Date.now();
  
  const interactionTime = endTime - startTime;
  expect(interactionTime).toBeLessThan(100); // Good FID < 100ms
});
```

### Network Performance Analysis

**Monitoring Network Requests**:
```javascript
test('analyzes network performance', async ({ page }) => {
  const networkMetrics = [];
  
  page.on('response', response => {
    networkMetrics.push({
      url: response.url(),
      status: response.status(),
      timing: response.timing(),
      size: response.headers()['content-length'],
    });
  });
  
  await page.goto('/dashboard');
  
  // Analyze performance
  const slowRequests = networkMetrics.filter(req => 
    req.timing.responseEnd - req.timing.requestStart > 1000
  );
  
  expect(slowRequests.length).toBe(0); // No requests over 1 second
  
  const totalSize = networkMetrics.reduce((sum, req) => 
    sum + parseInt(req.size || '0'), 0
  );
  expect(totalSize).toBeLessThan(2 * 1024 * 1024); // Under 2MB total
});
```

### Performance Optimization Techniques

**Resource Blocking for Faster Tests**:
```javascript
test('optimizes test speed by blocking resources', async ({ page }) => {
  // Block non-essential resources
  await page.route('**/*.{png,jpg,jpeg,gif,svg,css}', route => {
    route.abort(); // Skip images and styles for faster execution
  });
  
  await page.route('**/analytics/**', route => {
    route.abort(); // Skip analytics scripts
  });
  
  await page.goto('/app');
  
  // Test runs faster without loading heavy resources
  await page.click('#submit');
  await expect(page.locator('.success')).toBeVisible();
});
```

**Testing with Throttled Networks**:
```javascript
test('performs well on slow networks', async ({ page, context }) => {
  // Simulate slow 3G connection
  await context.route('**/*', async route => {
    // Add 500ms delay to simulate slow network
    await new Promise(resolve => setTimeout(resolve, 500));
    await route.continue();
  });
  
  const startTime = Date.now();
  await page.goto('/app');
  await page.waitForLoadState('networkidle');
  const loadTime = Date.now() - startTime;
  
  // App should load within reasonable time even on slow network
  expect(loadTime).toBeLessThan(5000);
});
```

### Memory and CPU Monitoring

**Performance Profiling**:
```javascript
test('monitors memory usage', async ({ page }) => {
  await page.goto('/memory-intensive-app');
  
  // Get initial memory usage
  const initialMemory = await page.evaluate(() => {
    return performance.memory?.usedJSHeapSize || 0;
  });
  
  // Perform memory-intensive operations
  await page.click('#load-large-dataset');
  await page.waitForTimeout(2000);
  
  // Check memory after operation
  const finalMemory = await page.evaluate(() => {
    return performance.memory?.usedJSHeapSize || 0;
  });
  
  const memoryIncrease = finalMemory - initialMemory;
  const memoryIncreaseMB = memoryIncrease / (1024 * 1024);
  
  // Memory increase should be reasonable
  expect(memoryIncreaseMB).toBeLessThan(50); // Less than 50MB increase
});
```

### Bundle Size Analysis

**Analyzing JavaScript Bundle Size**:
```javascript
test('validates bundle size', async ({ page }) => {
  const resourceSizes = new Map();
  
  page.on('response', async response => {
    if (response.url().endsWith('.js')) {
      const buffer = await response.body();
      resourceSizes.set(response.url(), buffer.length);
    }
  });
  
  await page.goto('/app');
  
  // Check main bundle size
  const mainBundle = Array.from(resourceSizes.entries())
    .find(([url]) => url.includes('main'));
    
  if (mainBundle) {
    const sizeKB = mainBundle[1] / 1024;
    expect(sizeKB).toBeLessThan(500); // Main bundle under 500KB
  }
});
```

## Question

What type of performance testing is most suitable for Playwright?

## Options

- **A.** High-volume load testing with thousands of concurrent users
- **B.** Database query optimization and indexing
- **C.** Web application user experience metrics like Core Web Vitals
- **D.** Server hardware stress testing

## Correct Answer

**C**

## Explanation

## Why Option C is Correct

**Option C** "Web application user experience metrics like Core Web Vitals" represents the most suitable performance testing for Playwright.

### Playwright's Performance Testing Strengths

Playwright excels at measuring **user-centric performance metrics** that directly impact user experience:

**Core Web Vitals Measurement**:
```javascript
test('measures real user experience', async ({ page }) => {
  await page.goto('/product-page');
  
  // Largest Contentful Paint (LCP)
  const lcp = await page.evaluate(() => {
    return new Promise(resolve => {
      new PerformanceObserver(list => {
        const entries = list.getEntries();
        resolve(entries[entries.length - 1].startTime);
      }).observe({ type: 'largest-contentful-paint', buffered: true });
    });
  });
  
  expect(lcp).toBeLessThan(2500); // Good UX threshold
});
```

**Real Browser Environment**:
```javascript
// Playwright measures performance in actual browsers
// - Real DOM rendering
// - Actual JavaScript execution
// - True network conditions
// - Genuine user interactions

test('interaction performance', async ({ page }) => {
  await page.goto('/app');
  
  const startTime = performance.now();
  await page.click('#interactive-button');
  await expect(page.locator('.response')).toBeVisible();
  const endTime = performance.now();
  
  expect(endTime - startTime).toBeLessThan(100); // First Input Delay
});
```

### User Experience Focus

**Frontend Performance Metrics**:
```javascript
// What Playwright measures well:
// ✅ Page load times
// ✅ Time to interactive
// ✅ Layout shift (CLS)
// ✅ Resource loading performance
// ✅ JavaScript execution time
// ✅ Network request timing

test('cumulative layout shift', async ({ page }) => {
  await page.goto('/dynamic-content');
  
  const cls = await page.evaluate(() => {
    return new Promise(resolve => {
      let clsValue = 0;
      new PerformanceObserver(list => {
        for (const entry of list.getEntries()) {
          if (!entry.hadRecentInput) {
            clsValue += entry.value;
          }
        }
        resolve(clsValue);
      }).observe({ type: 'layout-shift', buffered: true });
    });
  });
  
  expect(cls).toBeLessThan(0.1); // Good CLS score
});
```

### Analysis of Other Options:

**❌ Option A** "High-volume load testing with thousands of concurrent users":
```javascript
// Playwright isn't designed for load testing
// Each browser instance is resource-intensive
// Use tools like JMeter, Artillery, or k6 for load testing

// Playwright browser instance ≈ 50-100MB memory per test
// 1000 concurrent users = 50-100GB memory (impractical)
```

**❌ Option B** "Database query optimization and indexing":
```javascript
// Database performance testing requires direct DB connections
// Use database-specific tools, not browser automation

// Example of what Playwright CAN'T do:
// EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';
// Index analysis, query plan optimization
```

**❌ Option D** "Server hardware stress testing":
```javascript
// Hardware stress testing requires system-level tools
// CPU/memory/disk stress testing tools like:
// - stress-ng
// - sysbench  
// - Apache Bench for server stress

// Playwright tests web applications, not server hardware
```

### 🎯 Sweet Spot
Playwright excels at **frontend performance testing** - measuring how fast and responsive your web application feels to real users.
