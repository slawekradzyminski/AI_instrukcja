# Playwright Pre-Course Refresher

## Course
- **Title:** Playwright Fundamentals for Test Engineers
- **Difficulty:** intermediate
- **Estimated Time:** 60 minutes
- **Tags:** playwright, e2e-testing, automation

## Description
Master essential Playwright concepts for modern test automation including UI testing, API testing, and cross-browser automation

## Playwright Fundamentals for Test Engineers

Playwright is a modern end-to-end (E2E) testing framework from Microsoft that enables fast, reliable browser automation. It's an open-source tool that lets you simulate real user interactions for web applications across all major browsers. Playwright stands out by providing true cross-browser support (Chromium, Firefox, and WebKit) with a single consistent API, and it's accessible from multiple languages – including first-class support for TypeScript. For test engineers coming from other E2E tools (whether web-based or mobile), Playwright offers a robust, developer-friendly approach to automation, addressing many limitations of older frameworks.

### Cross-Browser and Cross-Platform Automation

One of Playwright's core strengths is its cross-browser automation capabilities. With Playwright, you can write one set of tests that run on Chromium (e.g. Google Chrome, Microsoft Edge), WebKit (Safari), and Firefox without any changes. This means you can confidently test your web app on all modern engines with the same code.

Playwright also supports all major operating systems (Windows, macOS, and Linux) and can run tests in various environments – locally or on CI servers, in headless mode (no UI) or headed mode (with a visible browser UI). This flexibility ensures your tests can mimic real user conditions, whether you're running quick headless checks in a pipeline or debugging a test visually on your machine.

#### Mobile Web Testing

Additionally, Playwright treats mobile web testing as a first-class citizen. It can emulate mobile devices, allowing you to test responsive web apps or mobile-specific behaviors. For example, Playwright provides native emulation for Google Chrome on Android and Mobile Safari, complete with device viewports, touch events, geolocation, and other mobile settings. The same test that runs on a desktop browser can be run in a mobile-emulated context, which is invaluable for ensuring a consistent user experience across devices.

### API Testing and Network Control

Beyond UI interactions, Playwright can also be used for API testing and network control as part of your test suite. Playwright's test runner includes an APIRequestContext that lets you send HTTP requests directly from Node.js (TypeScript/JavaScript) without opening a browser page. This means you can write tests to call REST APIs of your application to verify backend functionality, seed test data, or check post-conditions after a UI workflow – all within the Playwright framework.

Common use cases include:
- Prepping server state before a UI test (e.g. creating a user via API)
- Asserting that certain API calls were made as a result of a user action
- Network interception and mocking features
- Stubbing out network requests or simulating slow connections

These capabilities enable a mix of UI and API testing – you can verify that the frontend and backend work together correctly, or test each in isolation as needed. By combining browser automation with API calls, Playwright provides a comprehensive testing toolset in one framework.

### Test Isolation with Browser Contexts and Fixtures

Playwright was designed with test isolation in mind, which leads to more reliable and debuggable tests. Each test executes in a fresh, isolated browser context – essentially a brand new incognito browser session with no cookies or local storage from previous tests. This means tests cannot interfere with each other's state.

#### Benefits of Test Isolation

- One test's data or logged-in session won't carry over into the next test
- Full isolation improves reproducibility and prevents cascading failures
- No need to manually reset state or worry about test order
- Each test starts with a clean slate

#### Browser Context Model

Under the hood, Playwright achieves this by creating a new Browser Context for each test by default. A Browser Context is like a separate browser profile – it has its own cookies, storage, and settings. When using the Playwright Test runner, you get this behavior automatically: the runner will launch a browser if needed, create a new context and page for each test, and clean them up when the test is done.

#### Fixtures System

Playwright's test runner provides convenient fixtures to manage these objects. In a TypeScript test file, you typically import Playwright's test function, which allows you to declare tests with an object parameter (often called `{ page }`, or `{ page, context, browser }`). These are built-in fixtures Playwright supplies:

- `{ page }` fixture gives you an isolated Page in a new context
- `{ context }` gives the BrowserContext itself
- `{ browser }` gives the Browser instance
- `{ request }` gives an APIRequestContext for API calls

This fixture system ensures each test has everything it needs and nothing more. By relying on fixtures and the context model, test engineers can focus on the test logic, trusting Playwright to handle setup/teardown and isolation.

### Parallelism and Test Reliability Features

Modern test suites need to run quickly and reliably. Playwright addresses this with built-in parallelism and a host of reliability features.

#### Parallel Execution

By default, Playwright Test will run multiple tests at the same time using worker processes – for example, it might run each spec file in parallel, speeding up the overall execution. Each worker process launches its own browser instance to run tests, so tests running in parallel are truly isolated at the process and browser level.

You can configure the level of parallelism (number of workers) easily, but even out-of-the-box, Playwright will detect the number of CPU cores and run tests concurrently to optimize run time.

#### Reliability Features

- **Test retries**: Configure tests to automatically re-run on failure
- **Context restart**: Restart any browser context or worker after a failure to ensure clean state
- **Trace files**: Capture trace files, screenshots, and videos on failures or on demand
- **HTML reports**: Generate reports showing test results across different browsers with attached media

The Playwright Trace Viewer allows you to time-travel through a failed test's execution, seeing screenshots at each step, console logs, network requests, and the DOM snapshot. This makes it much easier to understand why a test failed.

### Modern Automation Features: Auto-Waiting, Locators, and Assertions

A hallmark of Playwright is its web-first automation features, which drastically reduce flaky tests and boilerplate synchronization code.

#### Auto-Waiting

Auto-waiting is built into every interaction: Playwright will automatically wait for elements to be ready (visible, enabled, stable, etc.) before performing actions like clicks or types. This means you no longer need to insert manual delays or sleep calls in your tests.

For example, if you call `await page.click()` on a button, Playwright first ensures the button is:
- Attached to the DOM
- Visible to the user
- Not animating
- Enabled to receive events

If those conditions aren't met immediately, Playwright will wait up to a timeout for them – so your test inherently waits for the UI to be ready without extra code.

#### Web-First Assertions

Similarly, Playwright provides web-first assertions. These are assertion methods integrated with Playwright's waiting mechanism. For example, `await expect(element).toHaveText("Hello")` will automatically retry until the element's text becomes "Hello" (or a timeout is reached), rather than failing instantly if the text isn't yet there.

#### Locator System

Underpinning auto-wait is Playwright's powerful Locator system. A Locator in Playwright is an object that identifies element(s) on the page and is designed for reusability and reliability. Unlike traditional selectors that return elements immediately, a Locator in Playwright defers the element lookup until an action is performed, and it will automatically retry the lookup if the DOM changes.

Playwright's API includes high-level locator methods that encourage stable selectors:
- `page.getByRole()` - locate by accessibility role
- `page.getByText()` - locate by text content
- `page.getByTestId()` - locate by test ID
- `page.getByLabel()` - locate by ARIA label
- `page.getByPlaceholder()` - locate by placeholder text

These methods make it easier to target elements in a way that mirrors how users see the UI, which makes tests more resilient to layout or minor DOM changes.

#### Additional Modern Features

- **Trusted user events**: Uses the actual browser event loop, so events like clicks are indistinguishable from real user input
- **Multi-page support**: Handle multiple pages or tabs easily
- **Shadow DOM and iframe support**: Built-in support to pierce shadow DOM and iframes for complex web apps

### Writing Maintainable Tests in TypeScript

Playwright's support for TypeScript helps ensure your tests are not only reliable but also maintainable and easy to work with.

#### TypeScript Benefits

- Write tests in `.ts` files – Playwright will transpile and run them seamlessly
- Static typing and IntelliSense for the Playwright API
- Autocomplete methods and flag mistakes before running tests
- Safer refactoring as test suites grow
- Catch errors at compile time

#### Developer Experience

TypeScript's syntax and object-oriented features (classes, interfaces, etc.) can feel more familiar than raw JavaScript for developers coming from languages like Java or C#. You can organize your test code with classes or page object models if desired, and be confident that changes will be checked by the compiler.

Playwright's test runner and configuration natively understand TypeScript, so features like Playwright Test config, custom helpers, or global setup can all be authored in TypeScript with full support.

### Conclusion

Playwright provides a comprehensive, modern testing platform for web applications, and using it with TypeScript further enhances its robustness. Test engineers will find that Playwright's cross-browser capabilities, automatic waiting and reliable locators, isolation and parallelism features, and integration of API testing all contribute to faster, more reliable test automation.

Playwright is effectively redefining E2E testing with its modern APIs, cross-browser support, and powerful tooling – it's a framework built for speed, stability, and maintainability. By adopting Playwright with TypeScript, teams can ensure their tests are thorough, maintainable, and closely aligned with real user experiences, making it an ideal choice for the next generation of automated testing.
