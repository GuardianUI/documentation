# Page

This is a shortened version of the [Playwright Page documention](https://playwright.dev/docs/api/class-page) with methods that may be most frequently used when testing with GuardianTest.

The Page class provides methods to interact with a tab within a browser instance.



## Methods

### [evaluate](https://playwright.dev/docs/api/class-page#page-evaluate)

Returns the result of running a function in the page context.

#### Usage

You can pass an argument to the function or you can evaluate strings as functions.

```typescript
const result = await page.evaluate(([a, b]) => {
    return Promise.resolve(a + b);
}, [1, 2]);
console.log(result); // 3
```

```typescript
const result = await page.evaluate("1 + 2");
console.log(result); // 3
```



### [getByAltText](https://playwright.dev/docs/api/class-page#page-get-by-alt-text)

Locate elements by their alt text

#### Usage

```html
<img alt="GuardianUI Example">
```

```typescript
page.getByAltText("GuardianUI Example").click();
```



### [getByLabel](https://playwright.dev/docs/api/class-page#page-get-by-label)

Locate elements by their `label`, or `aria-label`

#### Usage

```html
<input aria-label="Token Input">
```

```typescript
page.getByLabel("Token Input").fill("100");
```



### [getByPlaceholder](https://playwright.dev/docs/api/class-page#page-get-by-placeholder)

Locate elements by their placeholder content

#### Usage

```html
<input type="text" placeholder="name">
```

```typescript
page.getByPlaceholder("name").fill("John");
```



### [getByTestId](https://playwright.dev/docs/api/class-page#page-get-by-test-id)

Locate element by the `data-testid`

#### Usage

```html
<button data-testid="submit">
```

```typescript
page.getByTestId("submit").click();
```



### [getByText](https://playwright.dev/docs/api/class-page#page-get-by-text)

Locate elements that have certain text

#### Usage

```html
<div>Hello</div>
```

```typescript
page.getByText("Hello");
```



### [getByTitle](https://playwright.dev/docs/api/class-page#page-get-by-title)

Locate elements that have a certain title attribute

#### Usage

```html
<span title="Test Title">
```

```typescript
page.getByTitle("Test Title");
```



### [goto](https://playwright.dev/docs/api/class-page#page-goto)

Navigate to a URL

#### Usage

```typescript
await page.goto("https://www.guardianui.com");
```



### [locator](https://playwright.dev/docs/api/class-page#page-locator)

This is a very commonly used method in tests. It returns an element locator object that can be used to perform actions on the page

#### Usage

```typescript
page.locator("button:has-text('Demo Button')");
await page.locator("text=Connect").click();
```



### [route](https://playwright.dev/docs/api/class-page#page-route)

Provides the ability to modify network requests. Every request matching a URL pattern will stall unless it's continued, fulfilled, or aborted.

#### Usage

```typescript
// Mock a Graph response
await page.route("https://gateway.thegraph.com/api/**", route => {
    route.fulfill({
        body: {
            data: {
                "mocked-data"
            }
        }
    });
});
```



### [screenshot](https://playwright.dev/docs/api/class-page#page-screenshot)

Takes a screenshot

#### Usage

```typescript
await page.screenshot();
await page.screenshot(options);
```



## Events

### [on("console")](https://playwright.dev/docs/api/class-page#page-event-console)

Emitted when the page calls either `console.log`, `console.error`, `console.warn`, or `console.dir`

#### Usage

```typescript
page.on("console", msg => {
    console.log(msg);
});
```
