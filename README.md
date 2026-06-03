# Hands-On Automated Testing with Playwright TypeScript

A comprehensive Playwright automation showcase using **TypeScript**, **Playwright Test**, **Page Object Model**, **fixtures**, **multi-browser projects**, **CI/CD**, **API testing**, **login/authentication testing**, **accessibility testing**, **visual regression testing**, **mobile emulation**, **file workflows**, **authentication state**, and real-world framework design patterns.

This repository is written as a practical tutorial guide for QA engineers, SDETs, developers, and automation engineers who want to build reliable Playwright automation frameworks with TypeScript.

**Written by Brian McCarthy**

---

## Table of Contents

| # | Module | Primary Coverage |
|---:|---|---|
| 1 | [Quick Setup Refresher - Installing Playwright and Dependencies](#1-quick-setup-refresher---installing-playwright-and-dependencies) | Installing Playwright, browsers, TypeScript, project setup, first test, command line, UI Mode, and package scripts. |
| 2 | [Advanced Selectors and Handling Dynamic Content](#2-advanced-selectors-and-handling-dynamic-content) | `getBy*` locators, fallback selectors, locator strategy, auto-waiting, custom waits, dialogs, iframes, Shadow DOM, and dynamic UI. |
| 3 | [Browser-Agnostic Testing Across Chromium, Firefox, and WebKit](#3-browser-agnostic-testing-across-chromium-firefox-and-webkit) | Multi-browser projects, browser-specific debugging, cross-browser compatibility, and test matrix design. |
| 4 | [AI-Powered Test Generation](#4-ai-powered-test-generation) | Playwright Codegen, AI-assisted generation, locator refinement, prompt strategy, and reliable script cleanup. |
| 5 | [Crafting Scalable Tests with the Fixture System](#5-crafting-scalable-tests-with-the-fixture-system) | Built-in fixtures, custom fixtures, worker fixtures, page object injection, lifecycle, and reusable test context. |
| 6 | [Test Parallelization and Performance Optimization](#6-test-parallelization-and-performance-optimization) | Workers, parallel execution, retries, sharding, storage state, isolation, performance tuning, and CI speed. |
| 7 | [Integrating Workflows with CI/CD Pipelines](#7-integrating-workflows-with-cicd-pipelines) | GitHub Actions, reports, artifacts, browser matrices, Docker, environment variables, and CI execution strategy. |
| 8 | [Headless Testing and Debugging](#8-headless-testing-and-debugging) | Headless/headful modes, Inspector, UI Mode, screenshots, videos, traces, console logs, and debugging workflow. |
| 9 | [Accessibility Testing with Playwright and axe-core](#9-accessibility-testing-with-playwright-and-axe-core) | axe-core integration, WCAG checks, accessibility assertions, manual accessibility support, and reporting. |
| 10 | [Setting Up Visual Regression Testing](#10-setting-up-visual-regression-testing) | Screenshot baselines, visual comparison, thresholds, masking dynamic elements, snapshots, and stable visual testing. |
| 11 | [Testing Mobile Web Experiences](#11-testing-mobile-web-experiences) | Device emulation, viewport testing, touch actions, mobile navigation, geolocation, permissions, and responsive testing. |
| 12 | [Testing Forms](#12-testing-forms) | Text inputs, dropdowns, checkboxes, radio buttons, date pickers, custom controls, validation messages, and negative tests. |
| 13 | [Handling File Uploads and Downloads](#13-handling-file-uploads-and-downloads) | Upload inputs, download events, artifact handling, file cleanup, and validation of file workflows. |
| 14 | [Security and Authentication](#14-security-and-authentication) | Login/logout, storage state, multi-role testing, session reuse, protected routes, and safe secret handling. |
| 15 | [Best Practices for Test Maintainability](#15-best-practices-for-test-maintainability) | Clean code, Page Object Model, fixtures, data factories, stable locators, test isolation, and readable assertions. |
| 16 | [Real-World Project - Testing an E-Commerce Website](#16-real-world-project---testing-an-e-commerce-website) | Project structure, authentication state, data factories, product search, cart, checkout, API setup, and end-to-end flows. |
| 17 | [Streamlining Playwright in Modern Development Workflows](#17-streamlining-playwright-in-modern-development-workflows) | Agile workflows, DevOps feedback loops, tags, smoke/regression strategy, cloud testing, and release quality gates. |
| 18 | [Community and Learning Resources](#18-community-and-learning-resources) | Playwright docs, automation growth path, framework practice checklist, and continued learning resources. |
| 19 | [API Tests with Playwright](#api-tests-with-playwright) | Playwright `request` fixture, GET/POST/PUT/PATCH/DELETE examples, status validation, JSON assertions, and API setup. |
| 20 | [Login Tests](#login-tests) | Valid login, invalid login, required field validation, logout, protected route, and role-based testing. |
| 21 | [Locator Information and Best Practices](#locator-information-and-best-practices) | Locator priority, accessible selectors, test IDs, CSS, XPath, Shadow DOM, iframes, dynamic elements, and anti-patterns. |
| 22 | [Build a Playwright Framework from Scratch](#build-a-playwright-framework-from-scratch) | Required files, folder structure, setup steps, config, page objects, fixtures, API helpers, data factories, and CI. |
| 23 | [Framework from Scratch vs Pre-Built Frameworks](#framework-from-scratch-vs-pre-built-frameworks) | When to build custom layers, when to keep Playwright out-of-the-box, and framework selection by scenario. |
| 24 | [Best and Most Popular Framework Patterns](#best-and-most-popular-framework-patterns) | Playwright Test, POM, fixtures, BDD, API-first, accessibility, visual, mobile, Docker, cloud, and enterprise patterns. |
| 25 | [Top 30 Technical Interview Questions with Code Examples](#top-30-technical-interview-questions-with-code-examples) | Interview Q&A for Playwright TypeScript plus Selenium Java comparison/code examples where relevant. |

---

## Languages & Technologies

| Technology | Usage |
|---|---|
| **TypeScript** | Main language for Playwright test specs, page objects, fixtures, data factories, helpers, and configuration. |
| **Playwright Test** | Browser automation, API testing, assertions, fixtures, reporters, traces, videos, screenshots, and projects. |
| **Node.js / npm** | Runtime and dependency management. |
| **GitHub Actions** | CI execution, artifact publishing, browser matrix builds, and pull request quality gates. |
| **axe-core** | Automated accessibility scanning. |
| **Docker** | Consistent execution container for CI/local parity. |
| **Faker or custom factories** | Dynamic test data generation for forms, checkout, users, and API setup. |
| **dotenv** | Local environment variables and safe runtime configuration. |

---

## Recommended Project Structure

```text
Playwright-Automation-Hero-TypeScript-Showcase/
├── README.md
├── package.json
├── playwright.config.ts
├── tsconfig.json
├── .env.example
├── .gitignore
├── .github/
│   └── workflows/
│       └── playwright.yml
├── tests/
│   ├── setup/
│   │   ├── customer-auth.setup.ts
│   │   └── admin-auth.setup.ts
│   ├── api/
│   │   ├── products.api.spec.ts
│   │   ├── users.api.spec.ts
│   │   └── orders.api.spec.ts
│   ├── auth/
│   │   ├── login.spec.ts
│   │   ├── logout.spec.ts
│   │   └── protected-routes.spec.ts
│   ├── ui/
│   │   ├── advanced-selectors.spec.ts
│   │   ├── forms.spec.ts
│   │   ├── files.spec.ts
│   │   └── ecommerce.spec.ts
│   ├── visual/
│   │   └── visual-regression.spec.ts
│   ├── accessibility/
│   │   └── accessibility.spec.ts
│   └── mobile/
│       └── mobile-navigation.spec.ts
├── pages/
│   ├── base.page.ts
│   ├── login.page.ts
│   ├── product.page.ts
│   ├── cart.page.ts
│   └── checkout.page.ts
├── fixtures/
│   ├── test.fixtures.ts
│   ├── auth.fixtures.ts
│   └── api.fixtures.ts
├── data/
│   ├── user.factory.ts
│   ├── product.factory.ts
│   └── checkout.factory.ts
├── utils/
│   ├── api-client.ts
│   ├── file-utils.ts
│   ├── accessibility.ts
│   └── environment.ts
├── storage-states/
│   ├── customer.json
│   └── admin.json
└── test-results/
```

---

# 1. Quick Setup Refresher - Installing Playwright and Dependencies

## Module Details

This module creates the Playwright TypeScript environment from scratch. It covers project initialization, dependency installation, browser installation, the first test, package scripts, and local execution through CLI and UI Mode.

## Install as a New Independent Project

```bash
mkdir playwright-automation-hero
cd playwright-automation-hero
npm init -y
npm init playwright@latest
npx playwright install
npx playwright test
npx playwright show-report
```

## Add Playwright to an Existing Front-End Project

```bash
npm install -D @playwright/test typescript dotenv
npx playwright install
mkdir -p tests pages fixtures data utils
```

## Example `package.json` Scripts

```json
{
  "scripts": {
    "test": "playwright test",
    "test:headed": "playwright test --headed",
    "test:ui": "playwright test --ui",
    "test:debug": "playwright test --debug",
    "test:chromium": "playwright test --project=chromium",
    "test:smoke": "playwright test --grep @smoke",
    "report": "playwright show-report",
    "codegen": "playwright codegen"
  }
}
```

## First Test Example

```ts
import { test, expect } from '@playwright/test';

test('home page has expected title @smoke', async ({ page }) => {
  await page.goto('https://example.com');
  await expect(page).toHaveTitle(/Example Domain/);
  await expect(page.getByRole('heading', { name: 'Example Domain' })).toBeVisible();
});
```

## Expected Result

```text
1 passed
HTML report generated in playwright-report/
```

## Key Takeaways

- `npm init playwright@latest` creates a ready-to-run project.
- `npx playwright install` downloads browser engines.
- Playwright tests are asynchronous and normally use `await`.
- `expect()` assertions auto-wait until the condition passes or times out.

---

# 2. Advanced Selectors and Handling Dynamic Content

## Module Details

This module teaches locator strategy. Playwright tests should prioritize user-facing and accessibility-friendly locators: `getByRole`, `getByLabel`, `getByText`, `getByPlaceholder`, `getByAltText`, `getByTitle`, and `getByTestId`. CSS and XPath are fallback tools for difficult or unsupported DOM structures.

## Accessible Locator Example

```ts
import { test, expect } from '@playwright/test';

test('login with accessible locators', async ({ page }) => {
  await page.goto('/login');

  await page.getByLabel('Email').fill('customer@example.com');
  await page.getByLabel('Password').fill('Password123!');
  await page.getByRole('button', { name: 'Sign in' }).click();

  await expect(page.getByRole('heading', { name: 'My Account' })).toBeVisible();
});
```

## Fallback Locator Example

```ts
test('uses fallback selectors for complex DOM', async ({ page }) => {
  await page.goto('/products');

  await page.locator('[data-test="product-card"]').first().click();
  await page.locator('css=.cart-button').click();
  await page.locator('//button[contains(., "Checkout")]').click();

  await expect(page.getByRole('heading', { name: 'Checkout' })).toBeVisible();
});
```

## Dynamic Content Example

```ts
test('waits for search results without hard waits', async ({ page }) => {
  await page.goto('/search');

  await page.getByPlaceholder('Search products').fill('laptop');
  await page.getByRole('button', { name: 'Search' }).click();

  const results = page.getByTestId('search-result');
  await expect(results.first()).toBeVisible();
  await expect(results).toHaveCount(5);
});
```

## Dialog, Iframe, and Shadow DOM Examples

```ts
test('handles confirmation dialog', async ({ page }) => {
  page.on('dialog', async dialog => {
    expect(dialog.message()).toContain('Are you sure');
    await dialog.accept();
  });

  await page.goto('/account');
  await page.getByRole('button', { name: 'Delete account' }).click();
  await expect(page.getByText('Account deleted')).toBeVisible();
});
```

```ts
test('interacts with iframe payment form', async ({ page }) => {
  await page.goto('/checkout');

  const paymentFrame = page.frameLocator('iframe[name="payment"]');
  await paymentFrame.getByLabel('Card number').fill('4111111111111111');
  await paymentFrame.getByLabel('Expiration').fill('12/30');
  await paymentFrame.getByLabel('CVC').fill('123');

  await page.getByRole('button', { name: 'Pay now' }).click();
  await expect(page.getByText('Payment submitted')).toBeVisible();
});
```

```ts
test('handles shadow dom search component', async ({ page }) => {
  await page.goto('/components');

  await page.locator('custom-search').getByPlaceholder('Search').fill('tablet');
  await page.locator('custom-search').getByRole('button', { name: 'Submit' }).click();

  await expect(page.getByText('Search submitted')).toBeVisible();
});
```

## Expected Result

The tests locate stable elements, handle dynamic content without fixed sleeps, interact with dialogs, operate inside iframes, and validate Shadow DOM components.

## Best Practices

- Avoid `waitForTimeout()` except temporary debugging.
- Prefer accessible locators before CSS/XPath.
- Use `frameLocator()` for iframes.
- Use `page.on('dialog')` before the action that triggers the dialog.

---

# 3. Browser-Agnostic Testing Across Chromium, Firefox, and WebKit

## Module Details

Cross-browser testing validates that the same user workflows function in Chromium, Firefox, and WebKit. This catches CSS, JavaScript, rendering, and browser behavior differences before release.

## Multi-Browser Configuration

```ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  retries: process.env.CI ? 2 : 0,
  reporter: [['html'], ['list']],
  use: {
    baseURL: process.env.BASE_URL ?? 'http://localhost:5173',
    trace: 'on-first-retry'
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },
    { name: 'webkit', use: { ...devices['Desktop Safari'] } }
  ]
});
```

## Cross-Browser Test

```ts
import { test, expect } from '@playwright/test';

test('product page works across browsers', async ({ page, browserName }) => {
  await page.goto('/products');

  await expect(page.getByRole('heading', { name: 'Products' })).toBeVisible();
  await page.getByTestId('product-card').first().click();
  await expect(page.getByRole('button', { name: 'Add to cart' })).toBeVisible();

  console.log(`Validated in ${browserName}`);
});
```

## Run Commands

```bash
npx playwright test --project=chromium
npx playwright test --project=firefox
npx playwright test --project=webkit
npx playwright test
```

## Expected Result

```text
chromium  passed
firefox   passed
webkit    passed
```

## Best Practices

- Avoid browser-specific test logic unless the product explicitly requires it.
- Debug browser-specific failures with headed mode and traces.
- Run a fast Chromium smoke suite on pull requests and broader cross-browser tests before release.

---

# 4. AI-Powered Test Generation

## Module Details

Playwright Codegen and AI tools can accelerate script creation, but generated code should be treated as a draft. Good automation still requires stable locators, meaningful assertions, clean test names, and framework refactoring.

## Codegen Command

```bash
npx playwright codegen https://example.com
```

## Generated Test Example

```ts
import { test, expect } from '@playwright/test';

test('recorded login flow', async ({ page }) => {
  await page.goto('https://example.com/login');
  await page.getByLabel('Email').fill('customer@example.com');
  await page.getByLabel('Password').fill('Password123!');
  await page.getByRole('button', { name: 'Sign in' }).click();
  await expect(page.getByRole('heading', { name: 'Dashboard' })).toBeVisible();
});
```

## Prompt to Refine AI-Generated Tests

```text
Refactor this Playwright test into a TypeScript Page Object Model.
Use getByRole/getByLabel locators where possible.
Add meaningful assertions after each major action.
Avoid waitForTimeout.
Make test data configurable through environment variables.
```

## Refined Page Object

```ts
import { Page, expect } from '@playwright/test';

export class LoginPage {
  constructor(private readonly page: Page) {}

  async goto() {
    await this.page.goto('/login');
    await expect(this.page.getByRole('heading', { name: 'Sign in' })).toBeVisible();
  }

  async login(email: string, password: string) {
    await this.page.getByLabel('Email').fill(email);
    await this.page.getByLabel('Password').fill(password);
    await this.page.getByRole('button', { name: 'Sign in' }).click();
  }
}
```

## Expected Result

The generated script is converted into maintainable framework code with stable locators, reusable actions, and meaningful assertions.

## Best Practices

- Use Codegen for discovery, not final production tests.
- Review AI-generated locators and assertions.
- Replace brittle selectors with accessible locators.
- Use AI to accelerate boilerplate, not to skip QA review.

---

# 5. Crafting Scalable Tests with the Fixture System

## Module Details

Fixtures are Playwright's dependency injection system. They supply built-in objects such as `page`, `context`, `browser`, `request`, `browserName`, `isMobile`, and `headless`. Custom fixtures can provide page objects, API clients, data factories, authenticated pages, and logging utilities.

## Custom Page Fixture

```ts
import { test as base, expect, Page } from '@playwright/test';

class ProductsPage {
  constructor(private readonly page: Page) {}

  async goto() {
    await this.page.goto('/products');
  }

  async addFirstProductToCart() {
    await this.page.getByTestId('product-card').first().getByRole('button', { name: 'Add to cart' }).click();
  }
}

type Fixtures = {
  productsPage: ProductsPage;
};

export const test = base.extend<Fixtures>({
  productsPage: async ({ page }, use) => {
    await use(new ProductsPage(page));
  }
});

export { expect };
```

## Test Using Fixture

```ts
import { test, expect } from '../fixtures/test.fixtures';

test('customer can add product to cart', async ({ productsPage, page }) => {
  await productsPage.goto();
  await productsPage.addFirstProductToCart();
  await expect(page.getByTestId('cart-count')).toHaveText('1');
});
```

## Worker-Scoped Fixture

```ts
import { test as base } from '@playwright/test';

export const test = base.extend<{}, { apiBaseUrl: string }>({
  apiBaseUrl: [async ({}, use) => {
    await use(process.env.API_URL ?? 'https://api.example.com');
  }, { scope: 'worker' }]
});
```

## Expected Result

Tests receive reusable, typed objects directly in the test function. Common setup is centralized and easier to maintain.

## Best Practices

- Use test-scoped fixtures for page objects.
- Use worker-scoped fixtures for expensive reusable setup.
- Keep fixtures small and composable.
- Merge fixture sets only when needed.

---

# 6. Test Parallelization and Performance Optimization

## Module Details

Parallel execution speeds up test feedback but requires isolated test data and independent tests. This module covers workers, sharding, retries, avoiding repeated login, and measuring slow tests.

## Configuration

```ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  fullyParallel: true,
  workers: process.env.CI ? 4 : undefined,
  retries: process.env.CI ? 2 : 0,
  timeout: 30_000,
  expect: { timeout: 5_000 },
  use: {
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure'
  }
});
```

## Parallel-Safe Test Data

```ts
import { test, expect } from '@playwright/test';

test('registers a unique user', async ({ page }) => {
  const email = `user-${Date.now()}-${Math.random().toString(16).slice(2)}@example.com`;

  await page.goto('/register');
  await page.getByLabel('Email').fill(email);
  await page.getByLabel('Password').fill('Password123!');
  await page.getByRole('button', { name: 'Create account' }).click();

  await expect(page.getByText(email)).toBeVisible();
});
```

## Sharding Commands

```bash
npx playwright test --shard=1/3
npx playwright test --shard=2/3
npx playwright test --shard=3/3
```

## Expected Result

Tests run faster without colliding over shared users, shared orders, shared carts, or shared database state.

## Best Practices

- Never share mutable test data across parallel tests.
- Use generated data or API setup.
- Run `--repeat-each` to expose flaky tests.
- Use sharding for very large suites.

---

# 7. Integrating Workflows with CI/CD Pipelines

## Module Details

CI/CD integration runs tests automatically on pull requests, commits, nightly schedules, or release branches. It should upload Playwright reports and failure artifacts.

## GitHub Actions Workflow

```yaml
name: Playwright Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    timeout-minutes: 30
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        browser: [chromium, firefox, webkit]

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npx playwright install --with-deps
      - run: npx playwright test --project=${{ matrix.browser }}
      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: playwright-report-${{ matrix.browser }}
          path: playwright-report/
          retention-days: 7
```

## Docker Command

```bash
docker run --rm -v $PWD:/work -w /work mcr.microsoft.com/playwright:v1.55.0-jammy npm test
```

## Expected Result

CI installs dependencies, installs browsers, runs tests, and uploads reports even when tests fail.

## Best Practices

- Run smoke tests on pull requests.
- Run full regression before release or nightly.
- Upload traces, screenshots, videos, and HTML reports.
- Use matrix builds for cross-browser validation.

---

# 8. Headless Testing and Debugging

## Module Details

Playwright runs headless by default in CI and can run headed locally for debugging. Debugging tools include UI Mode, Inspector, Trace Viewer, screenshots, videos, console logs, and network inspection.

## Commands

```bash
npx playwright test --headed
npx playwright test --debug
npx playwright test --ui
npx playwright test --trace on
npx playwright show-report
npx playwright show-trace test-results/trace.zip
```

## Debug Configuration

```ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  use: {
    headless: process.env.HEADED ? false : true,
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
    trace: 'retain-on-failure'
  }
});
```

## Console Log Capture

```ts
test('captures console errors', async ({ page }) => {
  const errors: string[] = [];
  page.on('console', message => {
    if (message.type() === 'error') errors.push(message.text());
  });

  await page.goto('/');
  expect(errors).toEqual([]);
});
```

## Expected Result

A failed test includes trace, screenshot, video, and console evidence to determine whether the issue is test code, app behavior, data, or environment.

---

# 9. Accessibility Testing with Playwright and axe-core

## Module Details

Accessibility testing verifies that the app supports users with disabilities and follows accessibility rules. Automated scanning catches many issues, but manual keyboard and screen reader checks are still needed.

## Install

```bash
npm install -D @axe-core/playwright
```

## Accessibility Test

```ts
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test('home page has no critical accessibility violations', async ({ page }) => {
  await page.goto('/');

  const results = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa', 'wcag21aa'])
    .analyze();

  const criticalViolations = results.violations.filter(v => v.impact === 'critical');
  expect(criticalViolations).toEqual([]);
});
```

## Expected Result

The test fails when critical accessibility violations are detected.

## Best Practices

- Use `getByRole` and `getByLabel` to encourage accessible markup.
- Scan critical workflows such as login, checkout, and forms.
- Combine automated checks with manual keyboard testing.

---

# 10. Setting Up Visual Regression Testing

## Module Details

Visual regression testing compares screenshots to approved baselines. It catches layout regressions, missing content, broken styling, and responsive design issues.

## Page Screenshot

```ts
import { test, expect } from '@playwright/test';

test('home page visual baseline', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveScreenshot('home-page.png', {
    fullPage: true,
    maxDiffPixelRatio: 0.01
  });
});
```

## Component Screenshot

```ts
test('product card visual baseline', async ({ page }) => {
  await page.goto('/products');
  await expect(page.getByTestId('product-card').first()).toHaveScreenshot('product-card.png');
});
```

## Mask Dynamic Elements

```ts
await expect(page).toHaveScreenshot('checkout.png', {
  fullPage: true,
  mask: [page.getByTestId('timestamp'), page.getByTestId('ad-banner')]
});
```

## Expected Result

The first run creates a baseline. Later runs compare against that baseline and fail when visual differences exceed the threshold.

## Best Practices

- Mask dynamic dates, ads, IDs, animations, and random content.
- Prefer component-level screenshots for stability.
- Review diffs before updating baselines.

---

# 11. Testing Mobile Web Experiences

## Module Details

Playwright emulates mobile devices, touch input, viewport size, user agent, orientation, geolocation, and permissions. Mobile emulation is useful for responsive web validation but does not fully replace real-device testing.

## Mobile Project Configuration

```ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  projects: [
    { name: 'Mobile Chrome', use: { ...devices['Pixel 7'] } },
    { name: 'Mobile Safari', use: { ...devices['iPhone 14'] } }
  ]
});
```

## Mobile Navigation Test

```ts
test('mobile menu opens and navigates', async ({ page }) => {
  await page.goto('/');

  await page.getByRole('button', { name: 'Open menu' }).tap();
  await page.getByRole('link', { name: 'Products' }).tap();

  await expect(page).toHaveURL(/products/);
  await expect(page.getByRole('heading', { name: 'Products' })).toBeVisible();
});
```

## Expected Result

The test validates mobile-specific navigation and responsive page behavior under an emulated device profile.

---

# 12. Testing Forms

## Module Details

Forms are high-risk areas because they include user input, validation, error messages, required fields, dynamic controls, and submission workflows.

## Text Form Test

```ts
test('submits contact form', async ({ page }) => {
  await page.goto('/contact');

  await page.getByLabel('Full name').fill('Brian McCarthy');
  await page.getByLabel('Email').fill('brian@example.com');
  await page.getByLabel('Message').fill('Testing contact form with Playwright.');
  await page.getByRole('button', { name: 'Send message' }).click();

  await expect(page.getByText('Message sent successfully')).toBeVisible();
});
```

## Dropdowns, Checkboxes, and Validation

```ts
test('validates required form fields', async ({ page }) => {
  await page.goto('/register');
  await page.getByRole('button', { name: 'Create account' }).click();

  await expect(page.getByText('Email is required')).toBeVisible();
  await expect(page.getByText('Password is required')).toBeVisible();
});
```

```ts
test('fills preferences form', async ({ page }) => {
  await page.goto('/preferences');

  await page.getByLabel('Country').selectOption('US');
  await page.getByLabel('Subscribe to updates').check();
  await page.getByLabel('Email notifications').check();
  await page.getByRole('button', { name: 'Save preferences' }).click();

  await expect(page.getByText('Preferences saved')).toBeVisible();
});
```

## Expected Result

The tests validate both successful submission and proper validation errors.

---

# 13. Handling File Uploads and Downloads

## Module Details

File tests validate uploads, downloads, generated files, artifact names, and cleanup.

## Upload Test

```ts
import path from 'path';

test('uploads product image', async ({ page }) => {
  await page.goto('/admin/products/new');

  const filePath = path.join(process.cwd(), 'test-data', 'product-image.png');
  await page.getByLabel('Product image').setInputFiles(filePath);

  await expect(page.getByText('product-image.png')).toBeVisible();
});
```

## Download Test

```ts
test('downloads invoice PDF', async ({ page }) => {
  await page.goto('/orders/123');

  const downloadPromise = page.waitForEvent('download');
  await page.getByRole('link', { name: 'Download invoice' }).click();
  const download = await downloadPromise;

  expect(download.suggestedFilename()).toContain('invoice');
  await download.saveAs(`test-results/${download.suggestedFilename()}`);
});
```

## Expected Result

The upload test confirms the selected file is accepted. The download test confirms a file is generated and saved.

---

# 14. Security and Authentication

## Module Details

Authentication coverage validates login, logout, session reuse, protected route behavior, multiple roles, and secure test credential handling.

## Auth Setup with Storage State

```ts
import { test as setup, expect } from '@playwright/test';

setup('authenticate customer', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill(process.env.CUSTOMER_EMAIL!);
  await page.getByLabel('Password').fill(process.env.CUSTOMER_PASSWORD!);
  await page.getByRole('button', { name: 'Sign in' }).click();

  await expect(page.getByRole('heading', { name: 'My Account' })).toBeVisible();
  await page.context().storageState({ path: 'storage-states/customer.json' });
});
```

## Role-Based Test

```ts
test.use({ storageState: 'storage-states/customer.json' });

test('customer cannot access admin dashboard', async ({ page }) => {
  await page.goto('/admin');
  await expect(page.getByText('Access denied')).toBeVisible();
});
```

## Expected Result

Login runs once and tests reuse the saved authenticated state. Protected routes reject users without the required role.

---

# 15. Best Practices for Test Maintainability

## Module Details

Maintainability comes from clear tests, stable locators, reusable page objects, isolated data, concise fixtures, and meaningful assertions.

## Page Object Model Example

```ts
import { Page, expect } from '@playwright/test';

export class ProductPage {
  constructor(private readonly page: Page) {}

  async goto() {
    await this.page.goto('/products');
    await expect(this.page.getByRole('heading', { name: 'Products' })).toBeVisible();
  }

  async addProductToCartByName(productName: string) {
    const product = this.page.getByTestId('product-card').filter({ hasText: productName });
    await product.getByRole('button', { name: 'Add to cart' }).click();
  }

  async expectCartCount(count: number) {
    await expect(this.page.getByTestId('cart-count')).toHaveText(String(count));
  }
}
```

## Test Using POM

```ts
test('adds named product to cart', async ({ page }) => {
  const products = new ProductPage(page);

  await products.goto();
  await products.addProductToCartByName('Wireless Mouse');
  await products.expectCartCount(1);
});
```

## Expected Result

The test reads like a business workflow while selectors and page-specific actions are centralized in the page class.

---

# 16. Real-World Project - Testing an E-Commerce Website

## Module Details

A real-world e-commerce suite should cover product listing, search, product details, add to cart, cart updates, checkout, payment selection, order confirmation, login, logout, admin product management, API setup, and visual/accessibility checks.

## Checkout Test

```ts
import { test, expect } from '@playwright/test';
import { ProductPage } from '../pages/product.page';
import { CartPage } from '../pages/cart.page';

test('customer can add product and start checkout', async ({ page }) => {
  const products = new ProductPage(page);
  const cart = new CartPage(page);

  await products.goto();
  await products.addProductToCartByName('Wireless Mouse');
  await products.expectCartCount(1);

  await cart.goto();
  await cart.expectProductInCart('Wireless Mouse');
  await cart.checkout();

  await expect(page).toHaveURL(/checkout/);
});
```

## Test Data Factory

```ts
export function createCheckoutAddress() {
  return {
    street: '123 Testing Way',
    city: 'Tampa',
    state: 'FL',
    postalCode: '33602',
    country: 'USA'
  };
}
```

## Expected Result

The e-commerce test validates a revenue-critical workflow from product selection to checkout start.

---

# 17. Streamlining Playwright in Modern Development Workflows

## Module Details

Playwright fits into Agile and DevOps by giving fast feedback. Teams can run smoke tests on pull requests, full regression nightly, visual/accessibility checks before release, and targeted tags for feature branches.

## Tagged Tests

```ts
test('checkout smoke @smoke', async ({ page }) => {
  await page.goto('/checkout');
  await expect(page.getByRole('heading', { name: 'Checkout' })).toBeVisible();
});

test('full order workflow @regression', async ({ page }) => {
  await page.goto('/products');
  await page.getByTestId('product-card').first().getByRole('button', { name: 'Add to cart' }).click();
  await page.goto('/checkout');
  await expect(page.getByRole('button', { name: 'Place order' })).toBeVisible();
});
```

## Commands

```bash
npx playwright test --grep @smoke
npx playwright test --grep @regression
npx playwright test --project=chromium
npx playwright test --project="Mobile Chrome"
```

## Expected Result

Teams can run the right tests at the right time instead of always running the entire suite.

---

# 18. Community and Learning Resources

## Module Details

Playwright skills grow through repeated framework practice. Useful learning areas include official docs, release notes, locators, fixtures, API testing, storage state, traces, CI/CD, accessibility, visual testing, Docker, and cloud execution.

## Practice Checklist

```text
[ ] Build a first Playwright test
[ ] Add Page Object Model
[ ] Add fixtures
[ ] Add authentication storage state
[ ] Run Chromium, Firefox, and WebKit
[ ] Add GitHub Actions CI
[ ] Add screenshots, traces, and videos
[ ] Add accessibility checks
[ ] Add visual regression tests
[ ] Add mobile emulation tests
[ ] Add API setup helpers
[ ] Build a real e-commerce suite
```

---

# API Tests with Playwright

## Detailed Explanation

Playwright's `request` fixture supports API testing without opening a browser. API tests are useful for backend validation, test data setup, cleanup, and verifying integration behavior.

## GET Example

```ts
import { test, expect } from '@playwright/test';

test('GET products returns product list', async ({ request }) => {
  const response = await request.get(`${process.env.API_URL}/products`);
  expect(response.status()).toBe(200);

  const body = await response.json();
  expect(Array.isArray(body.data)).toBeTruthy();
  expect(body.data.length).toBeGreaterThan(0);
});
```

## POST Example

```ts
test('POST creates a product', async ({ request }) => {
  const response = await request.post(`${process.env.API_URL}/products`, {
    data: {
      name: 'Automation Hammer',
      price: 19.99,
      category: 'Tools'
    }
  });

  expect([200, 201]).toContain(response.status());
  const body = await response.json();
  expect(body.name).toBe('Automation Hammer');
});
```

## PUT/PATCH/DELETE Example

```ts
test('updates and deletes a product', async ({ request }) => {
  const create = await request.post(`${process.env.API_URL}/products`, {
    data: { name: 'Temporary Product', price: 10 }
  });
  const created = await create.json();

  const patch = await request.patch(`${process.env.API_URL}/products/${created.id}`, {
    data: { price: 12 }
  });
  expect([200, 204]).toContain(patch.status());

  const remove = await request.delete(`${process.env.API_URL}/products/${created.id}`);
  expect([200, 202, 204]).toContain(remove.status());
});
```

## Expected Result

API tests validate server behavior directly and can create clean test data for UI tests.

---

# Login Tests

## Valid Login

```ts
test('valid customer login', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill(process.env.CUSTOMER_EMAIL!);
  await page.getByLabel('Password').fill(process.env.CUSTOMER_PASSWORD!);
  await page.getByRole('button', { name: 'Sign in' }).click();

  await expect(page.getByRole('heading', { name: 'My Account' })).toBeVisible();
});
```

## Invalid Login

```ts
test('invalid login shows error', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill('wrong@example.com');
  await page.getByLabel('Password').fill('WrongPassword');
  await page.getByRole('button', { name: 'Sign in' }).click();

  await expect(page.getByText('Invalid email or password')).toBeVisible();
});
```

## Required Fields

```ts
test('login requires email and password', async ({ page }) => {
  await page.goto('/login');
  await page.getByRole('button', { name: 'Sign in' }).click();

  await expect(page.getByText('Email is required')).toBeVisible();
  await expect(page.getByText('Password is required')).toBeVisible();
});
```

## Expected Result

Login tests validate success, failure, required field validation, and protected route behavior.

---

# Locator Information and Best Practices

## Locator Priority

| Priority | Locator | Best Use |
|---:|---|---|
| 1 | `getByRole()` | Buttons, links, headings, checkboxes, radio buttons, dialogs. |
| 2 | `getByLabel()` | Inputs associated with labels. |
| 3 | `getByPlaceholder()` | Search boxes and simple fields. |
| 4 | `getByText()` | User-visible static text. |
| 5 | `getByTestId()` | Stable test hooks such as `data-test` or `data-testid`. |
| 6 | CSS | Complex or app-specific DOM targeting. |
| 7 | XPath | Text/relationship queries when CSS and accessible locators are not enough. |

## Locator Examples

```ts
await page.getByRole('button', { name: 'Submit Order' }).click();
await page.getByLabel('Email').fill('demo@example.com');
await page.getByPlaceholder('Search').fill('hammer');
await page.getByText('Payment was successful').isVisible();
await page.getByTestId('checkout-submit').click();
await page.locator('button.primary').click();
await page.locator('//button[contains(., "Submit")]').click();
```

## Anti-Patterns

```ts
// Avoid brittle index-based locators when possible
await page.locator('button').nth(7).click();

// Avoid fixed sleeps
await page.waitForTimeout(5000);

// Prefer this instead
await expect(page.getByText('Saved')).toBeVisible();
```

---

# Build a Playwright Framework from Scratch

## Required Files

| File / Folder | Required? | Purpose |
|---|---:|---|
| `package.json` | Yes | Dependencies and npm scripts. |
| `playwright.config.ts` | Yes | Main execution config. |
| `tsconfig.json` | Yes for TypeScript | TypeScript compiler and path aliases. |
| `tests/` | Yes | Test specs. |
| `tests/setup/*.setup.ts` | Recommended | Authentication and setup projects. |
| `pages/` | Recommended | Page Object Model classes. |
| `fixtures/` | Recommended | Custom fixture composition. |
| `data/` | Recommended | Data factories and test data. |
| `utils/` | Recommended | Shared utilities. |
| `.env.example` | Recommended | Environment variable template. |
| `.gitignore` | Yes | Exclude reports, traces, auth files, and secrets. |
| `.github/workflows/playwright.yml` | Recommended | CI/CD execution. |
| `Dockerfile` | Optional | Containerized execution. |

## Setup Commands

```bash
npm init -y
npm install -D @playwright/test typescript dotenv
npx playwright install
mkdir -p tests/setup tests/api tests/ui pages fixtures data utils
```

## Example Config

```ts
import { defineConfig, devices } from '@playwright/test';
import dotenv from 'dotenv';

dotenv.config();

export default defineConfig({
  testDir: './tests',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 4 : undefined,
  reporter: [['list'], ['html', { open: 'never' }]],
  use: {
    baseURL: process.env.BASE_URL ?? 'http://localhost:5173',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
    testIdAttribute: 'data-test'
  },
  projects: [
    { name: 'setup', testMatch: /.*\.setup\.ts/ },
    { name: 'chromium', use: { ...devices['Desktop Chrome'] }, dependencies: ['setup'] },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] }, dependencies: ['setup'] },
    { name: 'webkit', use: { ...devices['Desktop Safari'] }, dependencies: ['setup'] },
    { name: 'mobile-safari', use: { ...devices['iPhone 14'] }, dependencies: ['setup'] }
  ]
});
```

## Expected Result

A complete Playwright TypeScript framework exists with separate layers for tests, config, fixtures, page objects, API helpers, data factories, and CI.

---

# Framework from Scratch vs Pre-Built Frameworks

| Scenario | Use Playwright Out-of-the-Box | Build Custom Framework Layers |
|---|---|---|
| Learning Playwright | Yes | No |
| Small smoke suite | Yes | No |
| 10-30 tests | Yes, light POM only | Usually no |
| 50+ regression tests | No | Yes |
| Many repeated workflows | No | Yes, use POM/helpers |
| Multiple user roles | No | Yes, use storage states and role fixtures |
| API data setup needed | Maybe | Yes, add API helpers |
| CI/CD and reports required | Built-in config may be enough | Add reports/artifacts/matrix strategy |
| BDD required by stakeholders | No | Add Cucumber layer |
| Visual regression | Built-in screenshots may be enough | Add baselines/masking/review process |
| Enterprise suite | No | Yes, structured framework needed |

## Recommendation

Start simple with Playwright Test. Add Page Object Model, fixtures, API helpers, data factories, storage state, visual testing, and CI layers only when the suite grows enough to justify the maintenance overhead.

---

# Best and Most Popular Framework Patterns

| Framework Pattern | Best For | Why It Is Popular | Recommended Structure |
|---|---|---|---|
| Plain Playwright Test | Learning, POCs, small suites | Fast setup and minimal abstraction | `tests/*.spec.ts`, `playwright.config.ts` |
| Playwright + POM | Medium/large UI suites | Reusable page actions and selectors | `pages/`, `tests/` |
| Playwright + Fixtures | Shared setup and page injection | Native typed dependency injection | `fixtures/`, `pages/`, `tests/` |
| POM + Fixtures Hybrid | Professional frameworks | Clean tests and scalable structure | `pages/`, `fixtures/`, `data/`, `utils/` |
| API-First Playwright | Backend validation and UI setup | Uses built-in `request` fixture | `tests/api/`, `utils/api-client.ts` |
| BDD Playwright + Cucumber | Business-readable scenarios | Gherkin collaboration with BA/PO | `features/`, `steps/`, `pages/` |
| Visual Regression Framework | UI-heavy apps and design systems | Built-in screenshot assertions | `tests/visual/`, baseline snapshots |
| Accessibility Framework | WCAG/compliance checks | axe-core integrates well | `tests/accessibility/`, `utils/accessibility.ts` |
| Mobile Web Framework | Responsive/mobile workflows | Built-in device profiles | mobile projects in config |
| Dockerized Framework | CI consistency | Official Playwright Docker images | `Dockerfile`, CI workflow |
| Cloud Browser Framework | Enterprise scaling | Remote browsers and high parallelism | cloud config + CI secrets |

---

# Top 30 Technical Interview Questions with Code Examples

## 1. What is Playwright?

Playwright is a browser automation framework for Chromium, Firefox, and WebKit with built-in test runner, fixtures, assertions, traces, screenshots, videos, API testing, and browser contexts.

```ts
import { test, expect } from '@playwright/test';

test('basic page test', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveTitle(/Home/);
});
```

## 2. What is the Playwright `page` fixture?

`page` represents a browser tab.

```ts
test('uses page fixture', async ({ page }) => {
  await page.goto('/login');
});
```

## 3. What is a browser context?

A context is an isolated browser session with its own cookies, storage, permissions, and pages.

```ts
test('creates isolated context', async ({ browser }) => {
  const context = await browser.newContext();
  const page = await context.newPage();
  await page.goto('/');
  await context.close();
});
```

## 4. Best locator strategy?

Prefer user-facing locators before CSS/XPath.

```ts
await page.getByRole('button', { name: 'Login' }).click();
await page.getByLabel('Email').fill('demo@example.com');
```

## 5. How does auto-waiting work?

Playwright waits for elements to be actionable before actions and waits for assertions until timeout.

```ts
await expect(page.getByText('Saved')).toBeVisible();
```

## 6. How do you handle API testing?

Use the `request` fixture.

```ts
test('api status', async ({ request }) => {
  const response = await request.get(`${process.env.API_URL}/health`);
  expect(response.status()).toBe(200);
});
```

## 7. How do you reuse login?

Use storage state.

```ts
await page.context().storageState({ path: 'storage-states/customer.json' });
```

## 8. How do you run tests in parallel?

```ts
export default defineConfig({ fullyParallel: true, workers: 4 });
```

## 9. How do you debug failures?

```bash
npx playwright test --debug
npx playwright show-trace test-results/trace.zip
```

## 10. How do you handle iframes?

```ts
const frame = page.frameLocator('iframe[name="payment"]');
await frame.getByLabel('Card number').fill('4111111111111111');
```

## 11. How do you handle dialogs?

```ts
page.on('dialog', dialog => dialog.accept());
await page.getByRole('button', { name: 'Delete' }).click();
```

## 12. How do you handle downloads?

```ts
const downloadPromise = page.waitForEvent('download');
await page.getByText('Download').click();
const download = await downloadPromise;
```

## 13. How do you handle file uploads?

```ts
await page.getByLabel('Upload file').setInputFiles('test-data/file.pdf');
```

## 14. What is Page Object Model?

POM stores page actions and locators in classes.

```ts
class LoginPage {
  constructor(private page: Page) {}
  async login(email: string, password: string) {
    await this.page.getByLabel('Email').fill(email);
    await this.page.getByLabel('Password').fill(password);
    await this.page.getByRole('button', { name: 'Login' }).click();
  }
}
```

## 15. What are fixtures?

Fixtures provide reusable dependencies to tests.

```ts
export const test = base.extend<{ loginPage: LoginPage }>({
  loginPage: async ({ page }, use) => await use(new LoginPage(page))
});
```

## 16. How do you test visual regressions?

```ts
await expect(page).toHaveScreenshot('home-page.png');
```

## 17. How do you test accessibility?

```ts
const results = await new AxeBuilder({ page }).analyze();
expect(results.violations).toEqual([]);
```

## 18. How do you run mobile tests?

```ts
projects: [{ name: 'Mobile Safari', use: { ...devices['iPhone 14'] } }]
```

## 19. How do you mock APIs?

```ts
await page.route('**/api/products', route => route.fulfill({
  status: 200,
  body: JSON.stringify({ data: [] })
}));
```

## 20. How do you tag tests?

```ts
test('checkout smoke @smoke', async ({ page }) => { });
```

```bash
npx playwright test --grep @smoke
```

## 21. Selenium Java comparison: how do you open a browser?

```java
WebDriver driver = new ChromeDriver();
driver.get("https://example.com");
driver.quit();
```

## 22. Selenium Java: `findElement` vs `findElements`?

```java
WebElement one = driver.findElement(By.id("email"));
List<WebElement> many = driver.findElements(By.tagName("a"));
```

`findElement` throws if missing. `findElements` returns an empty list.

## 23. Selenium Java: explicit wait example

```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
WebElement button = wait.until(ExpectedConditions.elementToBeClickable(By.id("submit")));
button.click();
```

## 24. Selenium Java: iframe example

```java
driver.switchTo().frame("payment-frame");
driver.findElement(By.id("cardNumber")).sendKeys("4111111111111111");
driver.switchTo().defaultContent();
```

## 25. Selenium Java: Page Object Model example

```java
public class LoginPage {
    private WebDriver driver;
    private By email = By.id("email");

    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    public void enterEmail(String value) {
        driver.findElement(email).sendKeys(value);
    }
}
```

## 26. Selenium Java: screenshot example

```java
File src = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
```

## 27. Selenium Java: dropdown example

```java
Select select = new Select(driver.findElement(By.id("country")));
select.selectByVisibleText("United States");
```

## 28. Selenium Java: API test with REST Assured

```java
RestAssured.given()
    .baseUri("https://api.example.com")
.when()
    .get("/products")
.then()
    .statusCode(200);
```

## 29. When should you build a custom framework?

Build a framework when tests repeat setup, use multiple roles, require API data setup, need CI reporting, or will be maintained by multiple engineers.

## 30. What should be automated?

Automate stable, repeatable, high-value flows such as login, checkout, search, forms, API contracts, smoke tests, regression tests, mobile checks, accessibility, and visual regression.

---

## Common Playwright Commands

```bash
npx playwright test
npx playwright test --headed
npx playwright test --debug
npx playwright test --ui
npx playwright test --project=chromium
npx playwright test --grep @smoke
npx playwright show-report
npx playwright codegen https://example.com
```

---

## Portfolio Summary

This repository demonstrates professional Playwright automation using TypeScript. It covers installation, advanced locators, dynamic UI, dialogs, iframes, Shadow DOM, multi-browser testing, AI-assisted generation, custom fixtures, parallel execution, CI/CD, debugging, accessibility, visual regression, mobile emulation, forms, file uploads/downloads, authentication, Page Object Model, API testing, login tests, framework design, and interview preparation.

Written by Brian McCarthy
