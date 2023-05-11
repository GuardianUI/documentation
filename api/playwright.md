# Page

This is a shortened version of the [Playwright Page documention](https://playwright.dev/docs/api/class-page) with the methods that may be most frequently used when testing with the GuardianUI framework.

The Page class provides methods to interact with a tab within a browser instance.



## Methods

### close

Closes the page.

#### Usage

```typescript
await page.close();
```





### content

Gets the full HTML contents of the page.

#### Usage

```typescript
await page.content();
```



### evaluate

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



### getByAltText

Locate elements by their alt text

#### Usage

```html
<img alt="GuardianUI Example">
```

```typescript
await page.getByAltText("GuardianUI Example").click();
```



### getByLabel

Locate elements by their `label`, or `aria-label`

#### Usage

```html
<input aria-label="Token Input">
```

```typescript
await page.getByLabel("Token Input").fill("100");
```



### getByPlaceholder

Locate elements by their placeholder content

#### Usage

```html
<input type="text" placeholder="name">
```

```typescript
await page.getByPlaceholder("name").fill("John");
```



### getByTestId

Locate element by the `data-testid`

#### Usage

```html
<button data-testid="submit">
```

```typescript
await page.getByTestId("submit").click();
```



### getByText

Locate elements that have certain text

#### Usage

```html
<div>Hello</div>
```

```typescript
page.getByText("Hello");
```

left off at getByTitle
