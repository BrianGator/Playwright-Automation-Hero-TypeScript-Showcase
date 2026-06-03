# Hands-On Automated Testing with Playwright TypeScript

A comprehensive Playwright automation showcase using **TypeScript**, **Playwright Test**, **Page Object Model**, **fixtures**, **multi-browser projects**, **CI/CD**, **accessibility checks**, **visual regression testing**, **mobile emulation**, **file workflows**, **authentication state**, and a real-world **e-commerce testing project**.

This repository is written as a practical tutorial guide for QA engineers, SDETs, developers, and automation engineers who want to build reliable Playwright automation frameworks with TypeScript.

**Written by Brian McCarthy**

---

## Table of Contents

| # | Module | Primary Coverage |
|---|---|---|
| 1 | [Quick Setup Refresher - Installing Playwright and Dependencies](#1-quick-setup-refresher---installing-playwright-and-dependencies) | Installing Playwright, browsers, TypeScript, project setup, first test. |
| 2 | [Advanced Selectors and Handling Dynamic Content](#2-advanced-selectors-and-handling-dynamic-content) | `getBy*` locators, fallback selectors, dynamic waits, dialogs, iframes, Shadow DOM. |
| 3 | [Browser-Agnostic Testing Across Chromium, Firefox, and WebKit](#3-browser-agnostic-testing-across-chromium-firefox-and-webkit) | Multi-browser projects, browser-specific debugging, cross-browser compatibility. |
| 4 | [AI-Powered Test Generation](#4-ai-powered-test-generation) | Codegen, MCP/Copilot-assisted generation, prompt refinement, resilient locators. |
| 5 | [Crafting Scalable Tests with the Fixture System](#5-crafting-scalable-tests-with-the-fixture-system) | Built-in fixtures, custom fixtures, fixture scope, lifecycle, reusable test context. |
| 6 | [Test Parallelization and Performance Optimization](#6-test-parallelization-and-performance-optimization) | Workers, parallel execution, resource control, retries, sharding, performance tuning. |
| 7 | [Integrating Workflows with CI/CD Pipelines](#7-integrating-workflows-with-cicd-pipelines) | GitHub Actions, reports, artifacts, matrix builds, Dockerized execution. |
| 8 | [Headless Testing and Debugging](#8-headless-testing-and-debugging) | Headless/headful modes, Inspector, UI Mode, screenshots, videos, traces. |
| 9 | [Accessibility Testing with Playwright and axe-core](#9-accessibility-testing-with-playwright-and-axe-core) | axe-core integration, WCAG checks, automated accessibility tests, manual complement. |
| 10 | [Setting Up Visual Regression Testing](#10-setting-up-visual-regression-testing) | Screenshots, visual comparisons, thresholds, stable visual workflows. |
| 11 | [Testing Mobile Web Experiences](#11-testing-mobile-web-experiences) | Device emulation, viewport, touch, geolocation, mobile debugging. |
| 12 | [Testing Forms](#12-testing-forms) | Inputs, dropdowns, checkboxes, radio buttons, date pickers, validation messages. |
| 13 | [Handling File Uploads and Downloads](#13-handling-file-uploads-and-downloads) | Upload inputs, download events, file cleanup, test artifacts. |
| 14 | [Security and Authentication](#14-security-and-authentication) | Login/logout, storage state, multiple roles, session reuse, secure secrets. |
| 15 | [Best Practices for Test Maintainability](#15-best-practices-for-test-maintainability) | Clean code, Page Object Model, test data management, readable assertions. |
| 16 | [Real-World Project - Testing an E-Commerce Website](#16-real-world-project---testing-an-e-commerce-website) | Project initialization, structure, auth storage states, data factories, bulk users. |
| 17 | [Streamlining Playwright in Modern Development Workflows](#17-streamlining-playwright-in-modern-development-workflows) | Cloud testing, Agile workflows, DevOps feedback loops, scalable execution. |
| 18 | [Community and Learning Resources](#18-community-and-learning-resources) | Playwright docs, testing resources, automation growth path. |

---

## Languages & Technologies

| Technology | Usage |
|---|---|
| **TypeScript** | Main language for Playwright test specs, page objects, fixtures, and utilities. |
| **Playwright Test** | Test runner, browser automation engine, assertions, reporting, tracing. |
| **Node.js / npm** | Runtime and dependency management. |
| **GitHub Actions** | CI execution, artifact publishing, multi-browser matrix builds. |
| **axe-core** | Automated accessibility scanning. |
| **Docker** | Optional consistent execution container for CI. |
| **Faker** | Dynamic test data generation for e-commerce and form workflows. |
| **dotenv** | Environment-specific configuration and credentials. |

---

## Recommended Project Structure

```text
Playwright-Automation-TypeScript-Showcase/
├── README.md
├── package.json
├── playwright.config.ts
├── tsconfig.json
├── .github/
│   └── workflows/
│       └── playwright.yml
├── tests/
│   ├── setup/
│   │   └── auth.setup.ts
│   ├── advanced-selectors.spec.ts
│   ├── browser-agnostic.spec.ts
│   ├── accessibility.spec.ts
│   ├── visual-regression.spec.ts
│   ├── mobile.spec.ts
│   ├── forms.spec.ts
│   ├── files.spec.ts
│   ├── security-auth.spec.ts
│   └── ecommerce.spec.ts
├── pages/
│   ├── base.page.ts
│   ├── login.page.ts
│   ├── product.page.ts
│   ├── cart.page.ts
│   └── checkout.page.ts
├── fixtures/
│   ├── test.fixtures.ts
│   └── auth.fixtures.ts
├── data/
│   ├── user.factory.ts
│   └── product.factory.ts
├── utils/
│   ├── test-data.ts
│   ├── file-utils.ts
│   └── accessibility.ts
├── storage-states/
│   ├── customer.json
│   └── admin.json
└── test-results/
```

---

# 1. Quick Setup Refresher - Installing Playwright and Dependencies

## Module Details

This module establishes the local Playwright TypeScript environment. It covers creating a Node.js project, installing Playwright, downloading browser engines, adding TypeScript support, running the first test, and viewing the HTML report. This is the baseline for every later module.

## Install Commands

```bash
npm init -y
npm init playwright@latest
npx playwright install
npx playwright test
npx playwright show-report
```

## Example `package.json` Scripts

```json
{
  "scripts": {
    "test": "playwright test",
    "test:headed": "playwright test --headed",
    "test:ui": "playwright test --ui",
    "test:debug": "playwright test --debug",
    "report": "playwright show-report"
  }
}
```

## First Test Example

```ts
import { test, expect } from '@playwright/test';

test('home page has expected title', async ({ page }) => {
  await page.goto('https://example.com');
  await expect(page).toHaveTitle(/Example Domain/);
  await expect(page.getByRole('heading', { name: 'Example Domain' })).toBeVisible();
});
```

## Expected Output

```text
1 passed
HTML report generated in playwright-report/
```

## Expected Result

Playwright launches a browser, opens the target page, validates the page title, validates the heading, and records the test result.

## Key Takeaways

- `npm init playwright@latest` scaffolds a Playwright project.
- `npx playwright install` downloads browser engines.
- Playwright tests are asynchronous and use `await`.
- `expect()` provides web-first assertions that auto-wait.

---

# 2. Advanced Selectors and Handling Dynamic Content

## Module Details

This module covers robust locator strategy. The preferred approach is to use Playwright's accessibility-first locators such as `getByRole`, `getByLabel`, `getByText`, `getByPlaceholder`, `getByAltText`, `getByTitle`, and `getByTestId`. Fallback selectors such as CSS, XPath, and text-based selectors should be used only when the application does not expose accessible roles, labels, or stable test IDs.

Dynamic content should be handled with Playwright auto-waiting, locator assertions, and targeted custom waits only when necessary. This module also includes dialogs, iframes, nested frames, and Shadow DOM.

## Accessible Locators

```ts
import { test, expect } from '@playwright/test';

test('uses accessible getBy locators', async ({ page }) => {
  await page.goto('/login');

  await page.getByLabel('Email').fill('customer@example.com');
  await page.getByLabel('Password').fill('Password123!');
  await page.getByRole('button', { name: 'Sign in' }).click();

  await expect(page.getByRole('heading', { name: 'My Account' })).toBeVisible();
});
```

## Fallback Selectors

```ts
test('uses fallback selectors only when necessary', async ({ page }) => {
  await page.goto('/products');

  await page.locator('[data-test="product-card"]').first().click();
  await page.locator('css=.cart-button').click();
  await page.locator('//button[contains(., "Checkout")]').click();

  await expect(page.getByRole('heading', { name: 'Checkout' })).toBeVisible();
});
```

## Dynamic Content and Custom Waits

```ts
test('waits for dynamic search results', async ({ page }) => {
  await page.goto('/search');

  await page.getByPlaceholder('Search products').fill('laptop');
  await page.getByRole('button', { name: 'Search' }).click();

  const results = page.getByTestId('search-result');
  await expect(results.first()).toBeVisible();
  await expect(results).toHaveCount(5);
});
```

## Dialogs and Confirmations

```ts
test('handles confirmation dialog', async ({ page }) => {
  await page.goto('/account');

  page.on('dialog', async dialog => {
    expect(dialog.message()).toContain('Are you sure');
    await dialog.accept();
  });

  await page.getByRole('button', { name: 'Delete account' }).click();
  await expect(page.getByText('Account deleted')).toBeVisible();
});
```

## Iframes and Nested Frames

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

## Shadow DOM

```ts
test('handles shadow dom component', async ({ page }) => {
  await page.goto('/components');

  await page.locator('custom-search').getByPlaceholder('Search').fill('tablet');
  await page.locator('custom-search').getByRole('button', { name: 'Submit' }).click();

  await expect(page.getByText('Search submitted')).toBeVisible();
});
```

## Expected Result

The tests locate elements using stable, user-facing selectors first, handle dynamic UI without brittle sleeps, interact with dialogs, operate inside frames, and test Shadow DOM components.

## Key Takeaways

- Prefer `getByRole`, `getByLabel`, and other accessibility locators.
- Use CSS/XPath only as fallback strategies.
- Avoid fixed waits such as `waitForTimeout()` unless troubleshooting.
- Use `frameLocator()` for iframe workflows.
- Playwright pierces open Shadow DOM automatically through locators.

---

# 3. Browser-Agnostic Testing Across Chromium, Firefox, and WebKit

## Module Details

This module validates that the same tests work across Chromium, Firefox, and WebKit. Browser-agnostic testing catches compatibility problems before users encounter them. It also requires understanding browser-specific behavior, project configuration, and debugging cross-browser failures.

## Multi-Browser Configuration

```ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  retries: process.env.CI ? 2 : 0,
  reporter: [['html'], ['list']],
  use: {
    baseURL: 'https://example.com',
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

## Expected Output

```text
chromium  passed
firefox   passed
webkit    passed
```

## Expected Result

The same test suite runs across all configured browser engines and exposes any browser-specific failures.

## Key Takeaways

- Configure browsers as Playwright projects.
- Use `browserName` to log or branch only when absolutely necessary.
- Avoid browser-specific selectors and timing assumptions.
- Debug failures per browser using traces, screenshots, and headed mode.

---

# 4. AI-Powered Test Generation

## Module Details

This module covers AI-assisted Playwright test generation. Playwright Codegen can generate scripts by recording interactions. AI assistants such as Copilot can help draft test cases, create page objects, and generate assertions. Generated tests should always be reviewed, refactored, and hardened with resilient locators and clear assertions.

## Codegen Command

```bash
npx playwright codegen https://example.com
```

## Codegen-Style Output

```ts
import { test, expect } from '@playwright/test';

test('recorded login flow', async ({ page }) => {
  await page.goto('https://example.com/login');
  await page.getByLabel('Email').click();
  await page.getByLabel('Email').fill('customer@example.com');
  await page.getByLabel('Password').fill('Password123!');
  await page.getByRole('button', { name: 'Sign in' }).click();
  await expect(page.getByRole('heading', { name: 'Dashboard' })).toBeVisible();
});
```

## Prompt for AI-Generated Test Refinement

```text
Refactor this Playwright test into a TypeScript Page Object Model.
Use getByRole/getByLabel locators where possible.
Add meaningful assertions after each major action.
Avoid waitForTimeout.
Make test data configurable through environment variables.
```

## Refined Page Object Example

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

Generated scripts are converted into maintainable test code with strong locators, reusable page objects, and meaningful assertions.

## Key Takeaways

- Codegen is useful for discovery, not final framework design.
- AI-generated code must be reviewed before committing.
- Replace brittle selectors with accessible locators.
- Add assertions that prove business behavior.
- Keep generated code clean, typed, and maintainable.

---

# 5. Crafting Scalable Tests with the Fixture System

## Module Details

Fixtures are Playwright's dependency injection system. Built-in fixtures include `page`, `context`, `browser`, `request`, and `browserName`. Custom fixtures can provide page objects, authenticated users, test data, API clients, and environment-specific objects.

## Custom Fixture Example

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

## Test Using Custom Fixture

```ts
import { test, expect } from '../fixtures/test.fixtures';

test('customer can add product to cart', async ({ productsPage, page }) => {
  await productsPage.goto();
  await productsPage.addFirstProductToCart();

  await expect(page.getByTestId('cart-count')).toHaveText('1');
});
```

## Worker-Scoped Fixture Example

```ts
import { test as base } from '@playwright/test';

type WorkerFixtures = {
  apiToken: string;
};

export const test = base.extend<{}, WorkerFixtures>({
  apiToken: [async ({}, use) => {
    const token = process.env.API_TOKEN ?? 'demo-token';
    await use(token);
  }, { scope: 'worker' }]
});
```

## Expected Result

Fixtures reduce repeated setup, improve test readability, and make common dependencies available across test files.

## Key Takeaways

- Fixtures provide reusable setup and teardown.
- Test-scoped fixtures run per test.
- Worker-scoped fixtures run per worker.
- Custom fixtures are ideal for page objects, auth, API clients, and test data.

---

# 6. Test Parallelization and Performance Optimization

## Module Details

Playwright can run tests in parallel across workers. Parallelization improves feedback speed, but it requires isolated test data, independent tests, controlled resources, and stable environments. This module covers worker configuration, project-level parallelism, sharding, retries, and performance monitoring.

## Configuration Example

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
import { faker } from '@faker-js/faker';

test('registers a unique user', async ({ page }) => {
  const email = faker.internet.email().toLowerCase();

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

Tests run faster while remaining isolated and reliable.

## Key Takeaways

- Parallel tests must not share mutable state.
- Use unique data to avoid collisions.
- Retries should expose flakiness, not hide broken tests.
- Sharding helps large suites scale across CI jobs.

---

# 7. Integrating Workflows with CI/CD Pipelines

## Module Details

This module integrates Playwright with CI/CD. It covers GitHub Actions, installing dependencies, installing browsers, running tests, uploading reports, retaining artifacts, running browser/OS matrices, and using Docker for consistent execution.

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

## Expected Output

```text
npm ci completed
Playwright browsers installed
chromium passed
firefox passed
webkit passed
HTML report uploaded as artifact
```

## Expected Result

CI runs Playwright tests automatically and preserves reports for debugging.

## Key Takeaways

- CI should run the same command developers run locally.
- Upload reports, traces, screenshots, and videos as artifacts.
- Matrix builds validate multiple browsers.
- Docker provides consistent runtime dependencies.

---

# 8. Headless Testing and Debugging

## Module Details

Playwright runs headless by default in CI and can run headful locally for debugging. This module covers Inspector, UI Mode, trace viewer, screenshots, videos, and debug-friendly commands.

## Debug Commands

```bash
npx playwright test --headed
npx playwright test --debug
npx playwright test --ui
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

## Screenshot on Demand

```ts
import { test } from '@playwright/test';

test('captures checkout state', async ({ page }) => {
  await page.goto('/checkout');
  await page.screenshot({ path: 'test-results/checkout-page.png', fullPage: true });
});
```

## Expected Result

Failed tests generate screenshots, videos, and traces that can be reviewed locally or in CI artifacts.

## Key Takeaways

- Use headless mode in CI for speed.
- Use headed/debug/UI mode when developing or troubleshooting.
- Traces are the most complete debugging artifact.
- Screenshots and videos help document failure state.

---

# 9. Accessibility Testing with Playwright and axe-core

## Module Details

Accessibility testing validates whether pages follow accessibility rules such as labels, landmarks, color contrast, keyboard support, and ARIA usage. Automated tools such as axe-core can catch many issues, but manual testing is still required for keyboard flow, screen reader quality, and usability.

## Installation

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

## Targeted Accessibility Scan

```ts
test('checkout form is accessible', async ({ page }) => {
  await page.goto('/checkout');

  const results = await new AxeBuilder({ page })
    .include('[data-test="checkout-form"]')
    .analyze();

  expect(results.violations).toEqual([]);
});
```

## Expected Result

The test fails if axe-core finds accessibility violations matching the configured scope and severity.

## Key Takeaways

- Automated accessibility tests catch many WCAG issues.
- Use `getByRole` and `getByLabel` to encourage accessible markup.
- Include targeted scans for forms and critical workflows.
- Manual accessibility testing remains necessary.

---

# 10. Setting Up Visual Regression Testing

## Module Details

Visual regression testing compares screenshots against approved baselines. It helps catch layout shifts, styling regressions, missing images, responsive issues, and unintended UI changes. Stable visual tests require controlled data, deterministic viewport sizes, disabled animations when appropriate, and acceptable thresholds.

## Visual Test Example

```ts
import { test, expect } from '@playwright/test';

test('product card visual baseline', async ({ page }) => {
  await page.goto('/products');

  const productCard = page.getByTestId('product-card').first();
  await expect(productCard).toHaveScreenshot('product-card.png');
});
```

## Full Page Screenshot Comparison

```ts
test('home page visual baseline', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveScreenshot('home-page.png', {
    fullPage: true,
    maxDiffPixelRatio: 0.01
  });
});
```

## Disable Animations for Stable Screenshots

```ts
test.beforeEach(async ({ page }) => {
  await page.addStyleTag({
    content: `
      *, *::before, *::after {
        animation-duration: 0s !important;
        transition-duration: 0s !important;
      }
    `
  });
});
```

## Expected Output

```text
Snapshot written on first run
Later runs compare against baseline
Test fails if visual difference exceeds threshold
```

## Key Takeaways

- Keep visual tests stable and deterministic.
- Use component-level screenshots for less flaky checks.
- Control animations, dates, ads, random content, and test data.
- Review visual diffs before updating baselines.

---

# 11. Testing Mobile Web Experiences

## Module Details

Playwright can emulate mobile browsers using predefined device profiles. Mobile web tests should validate viewport behavior, touch interactions, responsive navigation, mobile menus, geolocation, permissions, and device-specific bugs. Emulation is valuable but does not fully replace real-device testing.

## Mobile Project Configuration

```ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  projects: [
    {
      name: 'Mobile Chrome',
      use: { ...devices['Pixel 7'] }
    },
    {
      name: 'Mobile Safari',
      use: { ...devices['iPhone 14'] }
    }
  ]
});
```

## Mobile Navigation Test

```ts
import { test, expect } from '@playwright/test';

test('mobile menu opens and navigates', async ({ page }) => {
  await page.goto('/');

  await page.getByRole('button', { name: 'Open menu' }).tap();
  await page.getByRole('link', { name: 'Products' }).tap();

  await expect(page).toHaveURL(/products/);
  await expect(page.getByRole('heading', { name: 'Products' })).toBeVisible();
});
```

## Geolocation Example

```ts
import { test, expect } from '@playwright/test';

test.use({
  geolocation: { latitude: 27.9506, longitude: -82.4572 },
  permissions: ['geolocation']
});

test('store locator uses geolocation', async ({ page }) => {
  await page.goto('/stores');
  await page.getByRole('button', { name: 'Use my location' }).click();
  await expect(page.getByText(/nearest store/i)).toBeVisible();
});
```

## Expected Result

Mobile tests validate responsive layout, touch interaction, mobile browser behavior, and location-based workflows.

## Key Takeaways

- Emulation is fast and useful for CI.
- Real-device testing is still needed for final confidence.
- Test mobile navigation and viewport-specific elements.
- Use device projects for repeatable mobile coverage.

---

# 12. Testing Forms

## Module Details

Forms are common sources of defects. This module covers text inputs, dropdowns, checkboxes, radio buttons, date pickers, custom fields, validation logic, error messages, success messages, and data persistence.

## Form Input Test

```ts
import { test, expect } from '@playwright/test';

test('submits contact form', async ({ page }) => {
  await page.goto('/contact');

  await page.getByLabel('Full name').fill('Brian McCarthy');
  await page.getByLabel('Email').fill('brian@example.com');
  await page.getByLabel('Message').fill('Testing contact form with Playwright.');
  await page.getByRole('button', { name: 'Send message' }).click();

  await expect(page.getByText('Message sent successfully')).toBeVisible();
});
```

## Dropdowns, Checkboxes, and Radio Buttons

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

## Validation Test

```ts
test('shows validation errors for invalid form', async ({ page }) => {
  await page.goto('/register');

  await page.getByRole('button', { name: 'Create account' }).click();

  await expect(page.getByText('Email is required')).toBeVisible();
  await expect(page.getByText('Password is required')).toBeVisible();
});
```

## Expected Result

The tests verify successful submission and validation behavior for required fields and invalid inputs.

## Key Takeaways

- Use labels for accessible form interaction.
- Test positive and negative form paths.
- Validate error messages, not only button clicks.
- Include custom controls and date picker workflows where applicable.

---

# 13. Handling File Uploads and Downloads

## Module Details

This module covers upload inputs, drag-and-drop style workflows, download events, file validation, and cleanup. File workflows should verify file name, extension, file contents when relevant, and whether the UI confirms upload/download success.

## File Upload Test

```ts
import { test, expect } from '@playwright/test';
import path from 'path';

test('uploads product image', async ({ page }) => {
  await page.goto('/admin/products/new');

  const filePath = path.join(process.cwd(), 'test-data', 'product-image.png');
  await page.getByLabel('Product image').setInputFiles(filePath);

  await expect(page.getByText('product-image.png')).toBeVisible();
});
```

## File Download Test

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

## Multiple File Upload

```ts
test('uploads multiple attachments', async ({ page }) => {
  await page.goto('/support');

  await page.getByLabel('Attachments').setInputFiles([
    'test-data/error-log.txt',
    'test-data/screenshot.png'
  ]);

  await expect(page.getByText('2 files selected')).toBeVisible();
});
```

## Expected Result

Upload tests confirm that selected files are accepted by the app. Download tests confirm that the expected file is produced and saved as a test artifact.

## Key Takeaways

- Use `setInputFiles()` for file uploads.
- Use `page.waitForEvent('download')` before clicking download links.
- Clean up generated files in CI.
- Validate file names and important file content when possible.

---

# 14. Security and Authentication

## Module Details

Authentication tests validate login, logout, session persistence, user role behavior, protected routes, and secure test credential handling. Playwright storage states allow authentication reuse so every test does not need to log in through the UI.

## Environment Variables

```env
BASE_URL=https://example.com
CUSTOMER_EMAIL=customer@example.com
CUSTOMER_PASSWORD=Password123!
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=Password123!
```

## Authentication Setup

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

## Authenticated Project Configuration

```ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  projects: [
    { name: 'setup', testMatch: /.*\.setup\.ts/ },
    {
      name: 'customer',
      dependencies: ['setup'],
      use: { storageState: 'storage-states/customer.json' }
    }
  ]
});
```

## Role-Based Test

```ts
import { test, expect } from '@playwright/test';

test('customer cannot access admin dashboard', async ({ page }) => {
  await page.goto('/admin');
  await expect(page.getByText('Access denied')).toBeVisible();
});
```

## Expected Result

Authentication is performed once during setup, session state is reused, and role-based access is validated safely.

## Key Takeaways

- Store secrets in environment variables or CI secrets.
- Do not commit real credentials or sensitive storage states.
- Use storage state to speed up authenticated tests.
- Test login, logout, expired sessions, and role restrictions.

---

# 15. Best Practices for Test Maintainability

## Module Details

Maintainable tests are readable, stable, isolated, and organized. This module covers Page Object Model, test data factories, reusable utilities, clear assertions, naming conventions, locator strategy, and avoiding anti-patterns.

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

## Test Using Page Object

```ts
import { test } from '@playwright/test';
import { ProductPage } from '../pages/product.page';

test('adds named product to cart', async ({ page }) => {
  const products = new ProductPage(page);

  await products.goto();
  await products.addProductToCartByName('Wireless Mouse');
  await products.expectCartCount(1);
});
```

## Test Data Factory

```ts
import { faker } from '@faker-js/faker';

export function createUser() {
  return {
    firstName: faker.person.firstName(),
    lastName: faker.person.lastName(),
    email: faker.internet.email().toLowerCase(),
    password: 'Password123!'
  };
}
```

## Expected Result

Tests become shorter, locators are centralized, test data is repeatable, and UI changes are easier to maintain.

## Key Takeaways

- Page objects should model user actions, not every DOM detail.
- Keep assertions meaningful and close to behavior.
- Avoid brittle selectors and fixed waits.
- Use factories for unique, realistic test data.

---

# 16. Real-World Project - Testing an E-Commerce Website

## Module Details

This module combines the full Playwright framework into a realistic e-commerce automation project. It covers project initialization, project structure, authentication storage states, data factories, product browsing, cart operations, checkout, admin workflows, and bulk user registration for load-style test data preparation.

## E-Commerce Page Objects

```ts
import { Page, expect } from '@playwright/test';

export class CartPage {
  constructor(private readonly page: Page) {}

  async goto() {
    await this.page.goto('/cart');
  }

  async expectProductInCart(productName: string) {
    await expect(this.page.getByTestId('cart-item').filter({ hasText: productName })).toBeVisible();
  }

  async checkout() {
    await this.page.getByRole('button', { name: 'Checkout' }).click();
  }
}
```

## Product-to-Checkout Test

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

## Bulk User Registration Data Factory

```ts
import { test, expect } from '@playwright/test';
import { faker } from '@faker-js/faker';

test('bulk user registration seed', async ({ page }) => {
  for (let index = 0; index < 5; index++) {
    const email = faker.internet.email().toLowerCase();

    await page.goto('/register');
    await page.getByLabel('First name').fill(faker.person.firstName());
    await page.getByLabel('Last name').fill(faker.person.lastName());
    await page.getByLabel('Email').fill(email);
    await page.getByLabel('Password').fill('Password123!');
    await page.getByRole('button', { name: 'Create account' }).click();

    await expect(page.getByText('Account created')).toBeVisible();
  }
});
```

## Expected Result

The e-commerce suite validates core revenue-critical flows: authentication, product browsing, cart, checkout, admin setup, and reusable test data generation.

## Key Takeaways

- Real projects need structure beyond individual test files.
- Authentication storage states improve speed.
- Data factories reduce brittle static test data.
- E-commerce tests should prioritize critical business flows.

---

# 17. Streamlining Playwright in Modern Development Workflows

## Module Details

This module explains how Playwright fits into Agile and DevOps workflows. Teams can run smoke tests on pull requests, regression tests nightly, cross-browser suites before release, and targeted tests during feature development. Cloud testing services can add browser/device scalability when local CI infrastructure is limited.

## Test Tagging Strategy

```ts
import { test, expect } from '@playwright/test';

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

## Run Commands by Workflow

```bash
npx playwright test --grep @smoke
npx playwright test --grep @regression
npx playwright test --project=chromium
npx playwright test --project="Mobile Chrome"
```

## Pull Request Feedback Loop

```yaml
name: Pull Request Smoke Tests
on: [pull_request]

jobs:
  smoke:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npx playwright install --with-deps chromium
      - run: npx playwright test --grep @smoke --project=chromium
```

## Expected Result

Playwright becomes part of the development workflow instead of a separate after-the-fact QA activity.

## Key Takeaways

- Use tags to separate smoke, regression, and feature tests.
- Run fast checks on pull requests.
- Run broader suites on scheduled or release workflows.
- Align Playwright coverage with Agile acceptance criteria and DevOps quality gates.

---

# 18. Community and Learning Resources

## Module Details

This final module identifies ongoing learning resources and broader testing topics. Playwright changes over time, so automation engineers should follow official documentation, release notes, GitHub issues, community examples, and general testing resources.

## Recommended Resources

- Playwright official documentation
- Playwright release notes
- Microsoft Playwright GitHub repository
- TypeScript documentation
- Web accessibility guidelines and WCAG references
- axe-core documentation
- CI/CD provider documentation
- Test automation design pattern resources

## Practice Roadmap

```text
[ ] Build a first Playwright test
[ ] Add Page Object Model
[ ] Add fixtures
[ ] Add authentication storage state
[ ] Run tests across Chromium, Firefox, and WebKit
[ ] Add GitHub Actions CI
[ ] Add screenshots, traces, and videos
[ ] Add accessibility checks
[ ] Add visual regression tests
[ ] Add mobile emulation tests
[ ] Build a real e-commerce suite
```

## Expected Result

The learner has a roadmap for continued Playwright growth and a framework structure that can be expanded into real project work.

## Key Takeaways

- Playwright skills grow through repeated framework practice.
- Official documentation should be checked regularly.
- Community patterns are useful, but framework decisions should fit the project.
- Strong automation combines reliable tests, maintainable architecture, and fast feedback.

---

# Common Playwright Commands

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

# Portfolio Summary

This repository demonstrates professional Playwright automation using TypeScript. It covers installation, advanced locators, dynamic UI, dialogs, iframes, Shadow DOM, multi-browser testing, AI-assisted test generation, custom fixtures, parallel execution, CI/CD, debugging, accessibility, visual regression, mobile emulation, forms, file uploads/downloads, authentication, Page Object Model, test data factories, and a real-world e-commerce testing workflow.

Written by Brian McCarthy
