# How many major browser engines does Playwright support natively?

## Theory

## Playwright's Browser Support

One of Playwright's strongest features is its comprehensive browser support. Unlike tools that focus on a single browser engine, Playwright supports all major browser engines with a unified API.

### Supported Browser Engines

**Chromium**: 
- Google Chrome
- Microsoft Edge  
- Brave Browser
- Any Chromium-based browser

**Firefox**:
- Mozilla Firefox
- Firefox ESR (Extended Support Release)

**WebKit**:
- Apple Safari (macOS)
- Safari on iOS (via mobile emulation)

### Cross-Browser Testing Made Easy

```javascript
// Same test runs on all browsers
test('button click works everywhere', async ({ page }) => {
  await page.goto('https://example.com');
  await page.click('#submit-button');
  await expect(page.locator('.success')).toBeVisible();
});
```

### Configuration Example

```javascript
// playwright.config.js
export default {
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },  
    { name: 'webkit', use: { ...devices['Desktop Safari'] } },
  ],
};
```

### Why This Matters

- **User Coverage**: Test on browsers your users actually use
- **Bug Detection**: Find browser-specific issues early
- **Confidence**: Deploy knowing your app works everywhere
- **Compliance**: Meet accessibility and compatibility requirements

### Mobile Browser Support

Playwright also supports mobile browser testing through device emulation:
```javascript
{ name: 'Mobile Chrome', use: { ...devices['Pixel 5'] } },
{ name: 'Mobile Safari', use: { ...devices['iPhone 12'] } },
```

## Question

How many major browser engines does Playwright support natively?

## Options

- **A.** 1 (Chromium only)
- **B.** 2 (Chromium and Firefox)
- **C.** 3 (Chromium, Firefox, WebKit)
- **D.** 4 (includes Internet Explorer)

## Correct Answer

**C**

## Explanation

## Why Option C is Correct

**Option C** "3 (Chromium, Firefox, WebKit)" is the correct answer.

Playwright natively supports exactly **three major browser engines**:

### 1. Chromium Engine
```javascript
// Tests Google Chrome, Microsoft Edge, Brave
{ name: 'chromium', use: { ...devices['Desktop Chrome'] } }
```

**Powers**: Google Chrome, Microsoft Edge, Brave, Opera, and many others

### 2. Firefox Engine  
```javascript
// Tests Mozilla Firefox
{ name: 'firefox', use: { ...devices['Desktop Firefox'] } }
```

**Powers**: Mozilla Firefox, Firefox ESR

### 3. WebKit Engine
```javascript  
// Tests Safari (macOS and mobile)
{ name: 'webkit', use: { ...devices['Desktop Safari'] } }
```

**Powers**: Apple Safari on macOS and iOS

### Complete Cross-Browser Coverage

These three engines cover **99%+ of web browser usage**:
```javascript
// One test, three engines
test('works everywhere', async ({ page }) => {
  // This runs on all three engines automatically
  await page.goto('/app');
  await expect(page.locator('h1')).toBeVisible();
});
```

### Analysis of Other Options:

**❌ Option A** "1": 
- Too limited - Playwright's strength is multi-browser support

**❌ Option B** "2":
- Missing one engine - Playwright supports all major engines

**❌ Option D** "4":
- No fourth major engine exists in the modern web

### 🎯 Key Benefit
With these 3 engines, Playwright tests cover virtually all desktop and mobile browser users worldwide.
