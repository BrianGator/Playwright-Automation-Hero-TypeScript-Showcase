# Hands On Automated Testing with Playwright

Unlock the full potential of Playwright, Microsoft's cutting-edge browser automation framework, with this comprehensive guide to modern web testing. Designed for developers and QA professionals, this repository provides practical examples, best practices, and real-world automation patterns.

From setting up your first test environment to mastering advanced techniques like AI-powered test generation, visual regression testing, and CI/CD integration, each chapter builds upon the last to equip you with the skills needed for professional test automation.

**Written by Brian McCarthy**

---

## Table of Contents

- [Languages & Technologies](#languages--technologies)
- [Methodologies & Patterns](#methodologies--patterns)
- [Project Functions & Features](#project-functions--features)
- [File Structure](#file-structure)
- [Project Overview](#project-overview)
- [Code Methodology Summary](#code-methodology-summary)
- [REST API Automation Guide](#rest-api-automation-guide)
- [Examples & Tutorials](#examples--tutorials)
- [Tips & Best Practices](#tips--best-practices)

---

## Languages & Technologies

### Primary Languages
| Language | Percentage | Usage |
|----------|-----------|-------|
| **TypeScript** | 96.8% | Core automation framework, test specs, page objects, fixtures |
| **Batchfile** | 2.7% | Build scripts and automation runners |
| **Other** | 0.5% | Configuration and documentation |

### Key Technologies & Dependencies
- **@playwright/test** (v1.55.0) - Modern browser automation framework
- **TypeScript** (v5.9.2) - Type-safe JavaScript for robust automation code
- **@faker-js/faker** (v10.0.0) - Test data generation
- **dotenv** (v17.2.2) - Environment configuration management
- **ts-node** (v10.9.2) - TypeScript execution for Node.js
- **@types/node** (v24.3.1) - Node.js type definitions

---

## Methodologies & Patterns

### 1. **Page Object Model (POM)**
The Page Object Model pattern encapsulates page-specific interactions into reusable classes, improving maintainability and reducing code duplication.

**Key Benefits:**
- Centralized element locators
- Abstracted user interactions
- Easy maintenance when UI changes
- Better code organization

**Example Structure:**
```
pages/
├── base.page.ts          # Base class with common functionality
├── product.page.ts       # Product-specific page interactions
└── login.page.ts         # Authentication page interactions
```

### 2. **Fixture-Based Authentication**
Custom Playwright fixtures provide pre-authenticated browser contexts, enabling efficient test execution with proper authentication state.

**Key Benefits:**
- Reusable authentication logic
- Dependency injection pattern
- Cleaner test code
- Performance optimization

### 3. **Setup/Teardown Pattern**
Dedicated setup tests manage authentication and prepare test data before running actual test scenarios.

### 4. **Test Data Factories**
Using @faker-js/faker for generating realistic test data dynamically.

### 5. **Configuration-Driven Testing**
Environment variables and configuration files control test behavior across different environments.

---

## Project Functions & Features

### Core Functions

| Function | Location | Purpose |
|----------|----------|---------|
| `gotoHome()` | BasePage | Navigate to application homepage |
| `selectCategory(category: string)` | BasePage | Select product category from navigation |
| `login(email: string, password: string)` | LoginPage | Authenticate user with credentials |
| `addToCartButton.click()` | ProductPage | Add product to shopping cart |
| `clearStorage(page: Page)` | BasePage | Clear local/session storage and cookies |
| `goto(productId?: string)` | ProductPage | Navigate to specific product page |
| `clickProductByIndex(index: number)` | ProductPage | Click product by position in list |

### Key Features

- **Browser Automation**: Full control over Chromium, Firefox, and WebKit browsers
- **Parallel Test Execution**: Run tests concurrently for faster feedback
- **Authentication Management**: Save and reuse authenticated sessions
- **Visual & Video Recording**: Capture screenshots and videos on test failures
- **Trace Recording**: Debug failed tests with detailed execution traces
- **HTML Reporting**: Comprehensive test reports with results visualization
- **CI/CD Integration**: Optimized for GitHub Actions and other CI platforms
- **Custom Test ID Attributes**: Target elements using `data-test` attributes

---

## File Structure

```
Playwright-Automation-TypeScript/
├── Chapter01-16/              # Individual chapter projects
│
├── Chapter16/
│   └── ecom-test-project/     # Complete e-commerce test automation project
│       ├── tests/
│       │   ├── auth.setup.ts                 # Authentication setup test
│       │   └── product.spec.ts               # Product functionality tests
│       │
│       ├── pages/
│       │   ├── base.page.ts                  # Base page class with common methods
│       │   ├── product.page.ts               # Product page object model
│       │   └── login.page.ts                 # Login page object model
│       │
│       ├── fixtures/
│       │   └── adminAuth.fixture.ts          # Custom authentication fixture
│       │
│       ├── data/
│       │   └── (test data factories)         # Dynamic test data generation
│       │
│       ├── playwright.config.ts              # Playwright configuration
│       ├── package.json                      # Project dependencies
│       ├── .env                              # Environment variables (not in repo)
│       └── .auth/                            # Stored authentication states
│           ├── customer1StorageState.json
│           └── adminStorageState.json
│
└── README.md                  # This file
```

### Key Directories Explained

#### `/tests` - Test Specifications
Contains Playwright test files (`.spec.ts`) that define test scenarios and `.setup.ts` files for test fixtures.

#### `/pages` - Page Object Models
Encapsulates page-specific element locators and interactions:
- **BasePage**: Common navigation and utility methods used by all pages
- **ProductPage**: Product browsing and cart operations
- **LoginPage**: Authentication workflows

#### `/fixtures` - Test Fixtures
Custom Playwright fixtures that extend the base `test` object with reusable, pre-configured browser contexts and pages.

#### `/data` - Test Data
Test data factories and utilities for generating realistic test data using libraries like faker.

---

## Project Overview

### What is This Project?

This repository is a comprehensive tutorial on professional Playwright automation testing. The main project is located in `Chapter16/ecom-test-project/`, which demonstrates a complete e-commerce test automation suite.

### What It Tests

The e-commerce test project automates testing for:
- User authentication (login/logout)
- Product browsing and category selection
- Shopping cart operations
- Product information display
- Navigation functionality

### Target Application

Tests are configured to run against: `https://practicesoftwaretesting.com`

---

## Code Methodology Summary

### 1. Page Object Model Pattern

**How It Works:**
```typescript
// pages/base.page.ts - Base class with shared functionality
export class BasePage {
  readonly page: Page;
  readonly navMenu: Locator;
  readonly navMenuHome: Locator;
  
  constructor(page: Page) {
    this.page = page;
    this.navMenu = page.getByTestId("nav-menu");
    this.navMenuHome = page.getByTestId("nav-home");
  }
  
  async gotoHome() {
    await this.page.goto("/");
  }
  
  async selectCategory(category: string) {
    await this.navMenuCategories.click();
    await this.navCategoryList.getByText(`${category}`).click();
  }
}
```

**Benefits:**
- All element locators in one place
- Easy to update when UI changes
- Reusable methods across tests
- Clear separation of concerns

### 2. Custom Fixtures for Authentication

**How It Works:**
```typescript
// fixtures/adminAuth.fixture.ts - Pre-authenticated page fixture
export const test = base.extend<{ adminAuthPage: Page }>({
  adminAuthPage: async ({ browser }, use) => {
    const storageStatePath = ".auth/adminStorageState.json";
    
    // Login and save authentication state
    const setupContext = await browser.newContext();
    const setupPage = await setupContext.newPage();
    const loginPage = new LoginPage(setupPage);
    await loginPage.goto();
    await loginPage.login(
      process.env.ADMIN_EMAIL!,
      process.env.ADMIN_PASSWORD!
    );
    
    await setupContext.storageState({ path: storageStatePath });
    await setupContext.close();
    
    // Provide authenticated page to tests
    const context = await browser.newContext({
      storageState: storageStatePath,
    });
    const page = await context.newPage();
    
    await use(page);
    await context.close();
  },
});
```

**Benefits:**
- Avoid repeated login in every test
- Faster test execution
- Clean separation of setup and test logic
- Reusable across multiple tests

### 3. Test Organization with Setup Tests

**How It Works:**
```typescript
// tests/auth.setup.ts - Create and save authentication state
setup("Create Customer 1 Authentication", async ({ page, context }) => {
  const email = process.env.CUSTOMER_1_EMAIL || "";
  const password = process.env.CUSTOMER_1_PASSWORD || "";
  const customer1AuthFile = ".auth/customer1StorageState.json";
  
  const loginPage = new LoginPage(page);
  await loginPage.gotoHome();
  await loginPage.navMenuSignIn.click();
  await loginPage.login(email, password);
  
  // Verify and save authentication
  await expect(loginPage.navMenu).toContainText("Jane Doe");
  await context.storageState({ path: customer1AuthFile });
});
```

### 4. Using Page Objects in Tests

**How It Works:**
```typescript
// tests/product.spec.ts - Clean, readable test using page objects
test("Add product to cart", async ({ page }) => {
  const productPage = new ProductPage(page);
  
  // Navigate
  await productPage.gotoHome();
  await productPage.selectCategory("Hand Tools");
  await productPage.clickProductByIndex(0);
  
  // Verify product information
  await expect(productPage.productName).toHaveText("Combination Pliers");
  
  // Perform action
  await productPage.addToCartButton.click();
  
  // Verify success
  await expect(productPage.addedToCartMessage).toHaveText(
    "Product added to shopping cart."
  );
  
  // Wait for message to disappear
  await expect(productPage.addedToCartMessage).not.toBeVisible({
    timeout: 10000,
  });
  
  // Verify cart updated
  await expect(productPage.navCartQuantity).toHaveText("1");
});
```

**Benefits:**
- Tests read like business requirements
- No implementation details visible
- Easy to understand what's being tested
- Simple to maintain and modify

### 5. Configuration-Driven Testing

**How It Works:**
```typescript
// playwright.config.ts - Centralized configuration
export default defineConfig({
  testDir: "./tests",
  fullyParallel: true,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  
  use: {
    trace: "on",
    baseURL: process.env.BASE_URL || "https://practicesoftwaretesting.com",
    testIdAttribute: "data-test",
    headless: true,
    video: "retain-on-failure",
    screenshot: "only-on-failure",
  },
  
  projects: [
    {
      name: "setup",
      testMatch: /.*\.setup\.ts/,
    },
    {
      name: "customer1-chromium",
      dependencies: ["setup"],
      use: {
        ...devices["Desktop Chrome"],
        storageState: ".auth/customer1StorageState.json",
      },
    },
  ],
});
```

**Benefits:**
- Single source of truth for configuration
- Easy to adjust for different environments
- Supports multiple browser/device combinations
- CI/CD optimizations built-in

---

## REST API Automation Guide

### Introduction

While Playwright is primarily a browser automation tool, it also supports HTTP API testing through the `APIRequestContext`. This guide shows how to automate REST API testing.

### 1. Basic API Request Setup

```typescript
import { test, expect } from "@playwright/test";

test("API: Verify product endpoint", async ({ request }) => {
  // GET request
  const response = await request.get("/products");
  
  // Verify response status
  expect(response.status()).toBe(200);
  
  // Parse and verify JSON response
  const data = await response.json();
  expect(data).toHaveProperty("products");
  expect(Array.isArray(data.products)).toBeTruthy();
});
```

### 2. POST Request with Request Body

```typescript
test("API: Create new product", async ({ request }) => {
  const response = await request.post("/products", {
    data: {
      name: "New Product",
      description: "Product description",
      price: 99.99,
      category: "Electronics",
    },
  });
  
  expect(response.status()).toBe(201);
  
  const createdProduct = await response.json();
  expect(createdProduct).toHaveProperty("id");
  expect(createdProduct.name).toBe("New Product");
});
```

### 3. Authentication in API Tests

```typescript
test("API: Authenticated endpoint", async ({ request }) => {
  const response = await request.get("/api/user/profile", {
    headers: {
      "Authorization": `Bearer ${process.env.API_TOKEN}`,
      "Content-Type": "application/json",
    },
  });
  
  expect(response.status()).toBe(200);
  const profile = await response.json();
  expect(profile.id).toBeDefined();
});
```

### 4. Handling Request/Response Headers

```typescript
test("API: Custom headers", async ({ request }) => {
  const response = await request.get("/api/data", {
    headers: {
      "X-Custom-Header": "CustomValue",
      "Accept-Language": "en-US",
    },
  });
  
  // Verify response headers
  expect(response.headers()["content-type"]).toContain("application/json");
});
```

### 5. Error Handling and Status Codes

```typescript
test("API: Handle error responses", async ({ request }) => {
  const response = await request.get("/api/products/invalid-id");
  
  // Don't throw on non-2xx status
  expect(response.status()).toBe(404);
  
  const error = await response.json();
  expect(error.message).toContain("Not found");
});
```

### 6. Combining Browser and API Testing

```typescript
test("API + UI: Create via API, verify in UI", async ({ page, request }) => {
  // Create product via API
  const apiResponse = await request.post("/products", {
    data: {
      name: "Test Product",
      price: 49.99,
    },
  });
  
  const newProduct = await apiResponse.json();
  
  // Verify in UI
  await page.goto(`/products/${newProduct.id}`);
  await expect(page.getByText("Test Product")).toBeVisible();
  await expect(page.getByText("$49.99")).toBeVisible();
});
```

### 7. API Test Fixture

```typescript
// fixtures/api.fixture.ts
import { test as base } from "@playwright/test";

export const test = base.extend<{ apiRequest: any }>({
  apiRequest: async ({ request }, use) => {
    const baseHeaders = {
      "Authorization": `Bearer ${process.env.API_TOKEN}`,
      "Content-Type": "application/json",
    };
    
    const api = {
      get: (url: string) => request.get(url, { headers: baseHeaders }),
      post: (url: string, data: any) =>
        request.post(url, { data, headers: baseHeaders }),
      put: (url: string, data: any) =>
        request.put(url, { data, headers: baseHeaders }),
      delete: (url: string) => request.delete(url, { headers: baseHeaders }),
    };
    
    await use(api);
  },
});

export { expect } from "@playwright/test";
```

### 8. Retry Logic for Flaky APIs

```typescript
test("API: Retry on failure", async ({ request }) => {
  let response;
  let retries = 3;
  
  while (retries > 0) {
    response = await request.get("/api/data");
    
    if (response.status() === 200) {
      break;
    }
    
    retries--;
    await new Promise(resolve => setTimeout(resolve, 1000));
  }
  
  expect(response?.status()).toBe(200);
});
```

---

## Examples & Tutorials

### Tutorial 1: Setting Up Your First Test

**Step 1: Install Dependencies**
```bash
cd Chapter16/ecom-test-project
npm install
npx playwright install
```

**Step 2: Create Environment File**
```bash
# .env file
CUSTOMER_1_EMAIL=customer@example.com
CUSTOMER_1_PASSWORD=password123
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=admin123
BASE_URL=https://practicesoftwaretesting.com
```

**Step 3: Run Tests**
```bash
# Run all tests
npx playwright test

# Run in headed mode (see browser)
npx playwright test --headed

# Run specific test file
npx playwright test product.spec.ts

# Run tests matching pattern
npx playwright test --grep "Add product"

# View detailed HTML report
npx playwright show-report
```

### Tutorial 2: Creating a New Page Object

```typescript
// pages/cart.page.ts
import { Locator, Page } from "@playwright/test";
import { BasePage } from "./base.page";

export class CartPage extends BasePage {
  readonly cartItems: Locator;
  readonly cartTotal: Locator;
  readonly checkoutButton: Locator;
  
  constructor(page: Page) {
    super(page);
    this.cartItems = page.locator(".cart-item");
    this.cartTotal = page.getByTestId("cart-total");
    this.checkoutButton = page.getByTestId("checkout");
  }
  
  async goto() {
    await this.page.goto("/cart");
  }
  
  async getItemCount() {
    return await this.cartItems.count();
  }
  
  async getTotalPrice() {
    const totalText = await this.cartTotal.textContent();
    return parseFloat(totalText?.replace(/[^\d.]/g, "") || "0");
  }
  
  async proceedToCheckout() {
    await this.checkoutButton.click();
  }
}
```

### Tutorial 3: Writing Your First Test

```typescript
// tests/cart.spec.ts
import { test, expect } from "@playwright/test";
import { CartPage } from "../pages/cart.page";
import { ProductPage } from "../pages/product.page";

test("Complete shopping flow", async ({ page }) => {
  // Initialize page objects
  const productPage = new ProductPage(page);
  const cartPage = new CartPage(page);
  
  // Browse products
  await productPage.gotoHome();
  await productPage.selectCategory("Hand Tools");
  await productPage.clickProductByIndex(0);
  await productPage.addToCartButton.click();
  
  // View cart
  await cartPage.goto();
  expect(await cartPage.getItemCount()).toBe(1);
  
  // Verify total
  const total = await cartPage.getTotalPrice();
  expect(total).toBeGreaterThan(0);
  
  // Proceed to checkout
  await cartPage.proceedToCheckout();
  await expect(page).toHaveURL(/.*checkout/);
});
```

### Tutorial 4: Using Test Data Factories

```typescript
// data/productFactory.ts
import { faker } from "@faker-js/faker";

export const createProductData = () => ({
  name: faker.commerce.productName(),
  description: faker.commerce.productDescription(),
  price: parseFloat(faker.commerce.price()),
  category: faker.commerce.department(),
  inStock: faker.datatype.boolean(),
});

// Usage in test
import { createProductData } from "../data/productFactory";

test("Create product with fake data", async ({ request }) => {
  const productData = createProductData();
  
  const response = await request.post("/products", {
    data: productData,
  });
  
  expect(response.status()).toBe(201);
  const created = await response.json();
  expect(created.name).toBe(productData.name);
});
```

---

## Tips & Best Practices

### 1. **Use Test IDs Instead of Selectors**
```typescript
// ✅ GOOD - Resilient to UI changes
const element = page.getByTestId("product-name");

// ❌ AVOID - Breaks with CSS changes
const element = page.locator("div.product > h1.title");
```

### 2. **Wait Intelligently**
```typescript
// ✅ GOOD - Wait for specific element
await expect(page.getByText("Success")).toBeVisible();

// ❌ AVOID - Fixed waits
await page.waitForTimeout(3000);
```

### 3. **Test from User Perspective**
```typescript
// ✅ GOOD - User-centric selectors
await page.getByLabel("Email").fill("test@example.com");
await page.getByRole("button", { name: "Login" }).click();

// ❌ AVOID - Implementation details
await page.locator("input#email_field_23").fill("test@example.com");
await page.locator("button.btn-primary.large").click();
```

### 4. **Handle Dynamic Content**
```typescript
// Use proper wait strategies
await page.waitForLoadState("networkidle");
await page.waitForSelector(".product-card", { state: "visible" });
await expect(page.locator(".product-card")).toHaveCount(10);
```

### 5. **Keep Tests Independent**
```typescript
// ✅ GOOD - Each test stands alone
test("User can add product to cart", async ({ page }) => {
  await page.goto("/");
  // Complete flow in single test
});

// ❌ AVOID - Dependencies between tests
test.describe.serial("Workflow", () => {
  test("First", () => { /* setup */ });
  test("Second", () => { /* depends on First */ });
});
```

### 6. **Use Meaningful Test Names**
```typescript
// ✅ GOOD
test("Verify user receives error message when submitting form with invalid email", () => {});

// ❌ AVOID
test("Test form", () => {});
```

### 7. **Leverage Parallelization**
```typescript
// Tests run in parallel by default in Playwright
// Control with:
test.describe.serial("Sequential tests", () => {
  test("First", () => {});
  test("Second", () => {});
});

test.describe("Parallel tests", () => {
  test("First", () => {});
  test("Second", () => {});
});
```

### 8. **Debug Failing Tests**
```bash
# Run with inspector
npx playwright test --debug

# Run single test with headed mode
npx playwright test --headed product.spec.ts

# Generate trace for analysis
npx playwright test --trace on

# View trace
npx playwright show-trace trace.zip
```

### 9. **Handle Timeouts Properly**
```typescript
test("Operation with extended timeout", async ({ page }) => {
  // Custom timeout for specific action
  await expect(page.getByText("Loading...")).not.toBeVisible({
    timeout: 30000, // 30 seconds
  });
  
  // Or set global timeout in config
  // expect.setDefaultTimeout(15000);
});
```

### 10. **Mobile Testing**
```typescript
import { devices } from "@playwright/test";

export default defineConfig({
  projects: [
    { name: "chromium", use: { ...devices["Desktop Chrome"] } },
    { name: "mobile", use: { ...devices["iPhone 12"] } },
    { name: "tablet", use: { ...devices["iPad Pro"] } },
  ],
});
```

### 11. **Visual Regression Testing**
```typescript
test("Product page layout", async ({ page }) => {
  await page.goto("/products/123");
  
  // Take screenshot for comparison
  await expect(page).toHaveScreenshot("product-page.png");
});

// Run with:
// npx playwright test --update-snapshots
```

### 12. **Environment-Specific Testing**
```typescript
const baseURL = process.env.ENV === "staging"
  ? "https://staging.example.com"
  : "https://production.example.com";

test("Cross-environment test", async ({ page }) => {
  await page.goto(baseURL);
  // Test runs on configured URL
});
```

---

## Running Tests

### Basic Commands

```bash
# Install dependencies
npm install
npx playwright install

# Run all tests
npx playwright test

# Run in headed mode (see browser)
npx playwright test --headed

# Run specific file
npx playwright test product.spec.ts

# Run tests matching pattern
npx playwright test --grep "@smoke"

# Run single test
npx playwright test -g "Add product to cart"

# Debug mode
npx playwright test --debug

# View test report
npx playwright show-report
```

### CI/CD Configuration

The `playwright.config.ts` is pre-configured for CI environments:
- Retries are enabled in CI
- Single worker in CI to avoid conflicts
- Videos captured on failure
- Screenshots captured on failure
- HTML reports generated

---

## Best Practices Summary

1. **Use Page Object Model** for maintainability
2. **Leverage Fixtures** for setup and reusable context
3. **Keep Tests Independent** - no test interdependencies
4. **Use Meaningful Assertions** - verify actual outcomes
5. **Handle Waits Properly** - use Playwright's auto-waiting
6. **Test from User Perspective** - use semantic selectors
7. **Maintain Test Data** - use factories for realistic data
8. **Document Test Purpose** - clear test descriptions
9. **Monitor Test Health** - track flaky tests
10. **Iterate and Improve** - refactor tests regularly

---

## Project Structure Links

- [Chapter 1-15](https://github.com/BrianGator/Playwright-Automation-TypeScript) - Individual learning modules
- [Chapter 16 - E-commerce Project](https://github.com/BrianGator/Playwright-Automation-TypeScript/tree/main/Chapter16/ecom-test-project) - Complete example project
  - [Tests](https://github.com/BrianGator/Playwright-Automation-TypeScript/tree/main/Chapter16/ecom-test-project/tests)
  - [Page Objects](https://github.com/BrianGator/Playwright-Automation-TypeScript/tree/main/Chapter16/ecom-test-project/pages)
  - [Fixtures](https://github.com/BrianGator/Playwright-Automation-TypeScript/tree/main/Chapter16/ecom-test-project/fixtures)

---

## Resources

- [Playwright Documentation](https://playwright.dev/)
- [Playwright API Reference](https://playwright.dev/docs/api/class-playwright)
- [Playwright Best Practices](https://playwright.dev/docs/best-practices)
- [Playwright Debugging](https://playwright.dev/docs/debug)
- [Practice Testing Website](https://practicesoftwaretesting.com)

---

## License

ISC

---

**Written by Brian McCarthy**

For questions, improvements, or contributions, please refer to the individual chapter directories for specific implementation details.
