# Which practice is considered a Playwright anti-pattern that should be avoided?

## Theory

## Best Practices and Anti-Patterns

Understanding Playwright best practices and common anti-patterns is crucial for building maintainable, reliable, and efficient test suites. These guidelines help teams avoid common pitfalls and maximize testing effectiveness.

### Test Design Best Practices

**Keep Tests Focused and Independent**:
```javascript
// ✅ Good: Focused test with single responsibility
test('user can add item to cart', async ({ page }) => {
  await page.goto('/products/1');
  await page.click('#add-to-cart');
  await expect(page.locator('.cart-count')).toHaveText('1');
});

// ❌ Bad: Testing multiple unrelated features
test('user workflow', async ({ page }) => {
  // Login, browse products, add to cart, checkout, 
  // update profile, write review... (too much in one test)
});
```

**Use Stable Locators**:
```javascript
// ✅ Good: Semantic, stable locators
await page.getByRole('button', { name: 'Submit Order' });
await page.getByLabel('Email Address');
await page.getByTestId('checkout-form');

// ❌ Bad: Fragile CSS selectors
await page.click('.btn.btn-primary.mt-3.px-4'); // Breaks when styles change
await page.locator('div:nth-child(3) > span').click(); // Breaks when structure changes
```

### Common Anti-Patterns to Avoid

**Anti-Pattern: Hard-coded Waits**:
```javascript
// ❌ Bad: Arbitrary timeouts
await page.click('#submit');
await page.waitForTimeout(3000); // Why 3 seconds? What if it takes 4?
const success = await page.isVisible('.success');

// ✅ Good: Smart waiting with web-first assertions
await page.click('#submit');
await expect(page.locator('.success')).toBeVisible(); // Waits as needed
```

**Anti-Pattern: Overly Complex Test Data**:
```javascript
// ❌ Bad: Complex, hard-to-maintain test data
const user = {
  personalInfo: { /* 20 fields */ },
  preferences: { /* 15 fields */ },
  history: { /* 100 records */ },
  // ... only testing login functionality
};

// ✅ Good: Minimal test data for the scenario
const user = { email: 'test@example.com', password: 'test123' };
```

### Performance and Reliability

**Optimize for Speed**:
```javascript
// ✅ Good: Block unnecessary resources in tests
await page.route('**/*.{png,jpg,jpeg,gif,svg}', route => route.abort());
await page.route('**/analytics/**', route => route.abort());

// ✅ Good: Use API for setup when possible
test('user dashboard shows correct data', async ({ page, request }) => {
  // Fast: Create data via API
  const user = await request.post('/api/users', { data: userData });
  
  // UI test focuses on presentation, not data creation
  await page.goto('/dashboard');
  await expect(page.locator('.welcome')).toContainText(user.name);
});
```

**Proper Error Handling**:
```javascript
// ✅ Good: Descriptive test names and clear assertions
test('shopping cart displays correct item count after adding product', async ({ page }) => {
  await page.goto('/products/laptop');
  await page.click('[data-testid="add-to-cart"]');
  
  await expect(page.locator('[data-testid="cart-count"]')).toHaveText('1');
  await expect(page.locator('.cart-summary')).toContainText('Laptop');
});

// ❌ Bad: Vague test names and generic assertions
test('test cart', async ({ page }) => {
  // ... unclear what this actually tests
  expect(something).toBeTruthy(); // Uninformative assertion
});
```

### Maintenance and Organization

**Use Page Object Pattern Properly**:
```javascript
// ✅ Good: Page objects encapsulate page-specific logic
export class CheckoutPage {
  constructor(page) {
    this.page = page;
    this.emailInput = page.getByLabel('Email');
    this.submitButton = page.getByRole('button', { name: 'Place Order' });
  }
  
  async fillCheckoutForm(customerInfo) {
    await this.emailInput.fill(customerInfo.email);
    // ... other form fields
  }
  
  async submitOrder() {
    await this.submitButton.click();
    await expect(this.page.locator('.confirmation')).toBeVisible();
  }
}

// ❌ Bad: Page objects that are too generic or complex
export class GenericPage {
  // Handles everything for every page (too broad)
  async doEverything() { /* ... */ }
}
```

### Testing Strategy

**Test Pyramid Approach**:
```javascript
// ✅ Good: Use E2E tests for critical user journeys
test('complete purchase flow works end-to-end', async ({ page }) => {
  // Test the most important business workflow
  await completeSuccessfulPurchase(page);
});

// ✅ Good: Use API tests for business logic
test('order calculation includes tax and shipping', async ({ request }) => {
  const response = await request.post('/api/orders/calculate', {
    data: { items: [{ id: 1, quantity: 2 }] }
  });
  const order = await response.json();
  expect(order.total).toBe(order.subtotal + order.tax + order.shipping);
});

// ❌ Bad: Testing every edge case with E2E tests
test('error message for invalid email format', async ({ page }) => {
  // This should be a unit test, not E2E
});
```

## Question

Which practice is considered a Playwright anti-pattern that should be avoided?

## Options

- **A.** Using getByRole() for element selection
- **B.** Using hard-coded timeouts instead of web-first assertions
- **C.** Running tests in parallel across multiple browsers
- **D.** Creating reusable page object classes

## Correct Answer

**B**

## Explanation

## Why Option B is Correct

**Option B** "Using hard-coded timeouts instead of web-first assertions" is a classic Playwright anti-pattern that should be avoided.

### The Problem with Hard-Coded Timeouts

**Arbitrary Waiting (Anti-Pattern)**:
```javascript
// ❌ Bad: Hard-coded timeouts
test('form submission', async ({ page }) => {
  await page.click('#submit');
  await page.waitForTimeout(3000);    // Why 3 seconds? Magic number!
  
  const isVisible = await page.isVisible('.success');
  expect(isVisible).toBe(true);
  
  // Problems:
  // - Fails if response takes 3.1 seconds
  // - Wastes time if response comes in 0.5 seconds  
  // - Arbitrary timeout values are unreliable
});
```

**Web-First Assertions (Best Practice)**:
```javascript
// ✅ Good: Smart waiting with auto-retry
test('form submission', async ({ page }) => {
  await page.click('#submit');
  
  // Automatically waits until condition is met (up to default timeout)
  await expect(page.locator('.success')).toBeVisible();
  
  // Benefits:
  // - Waits only as long as needed
  // - Retries automatically until success or timeout
  // - No arbitrary delays
  // - Self-documenting intent
});
```

### Why Hard-Coded Timeouts Are Problematic

**Flaky Tests**:
```javascript
// Network conditions vary between environments
await page.click('#load-data');
await page.waitForTimeout(2000);  // Works locally, fails in CI

// Better approach:
await page.click('#load-data');
await expect(page.locator('.data-loaded')).toBeVisible({ timeout: 10000 });
```

**Wasted Time**:
```javascript
// Unnecessary delays slow down test suite
await page.waitForTimeout(5000);  // Always waits 5 seconds
// vs
await expect(element).toBeVisible(); // Waits 0.1s if element appears quickly
```

### Analysis of Other Options:

**❌ Option A** "Using getByRole() for element selection":
```javascript
// This is actually a BEST PRACTICE, not an anti-pattern
await page.getByRole('button', { name: 'Submit' }); // ✅ Recommended
```

**❌ Option C** "Running tests in parallel across multiple browsers":
```javascript
// Parallel execution is a Playwright strength, not an anti-pattern
// playwright.config.js
projects: [
  { name: 'chromium' },
  { name: 'firefox' },   // ✅ Good practice
  { name: 'webkit' },
]
```

**❌ Option D** "Creating reusable page object classes":
```javascript
// Page Object Model is a best practice for maintainability
export class LoginPage {
  constructor(page) { this.page = page; }
  async login(email, password) { /* reusable logic */ }
}
// ✅ Good practice
```

### 🎯 Key Insight
Hard-coded timeouts represent **"dumb waiting"** while web-first assertions provide **"smart waiting"** - automatically adapting to actual application behavior rather than arbitrary delays.
