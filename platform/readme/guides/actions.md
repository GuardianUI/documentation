# Actions

Playwright can interact with HTML input elements easily. This is a shortened version of the [Playwright Actions documentation](https://playwright.dev/docs/input).



### [Text Input](https://playwright.dev/docs/input#text-input)

Use `locator.fill()` which focuses the element and trigger an `input` event with the desired text.

#### Usage

```typescript
await page.locator("input[class='name-input']").fill("Jeff");
```



### [Checkboxes and Radio Buttons](https://playwright.dev/docs/input#checkboxes-and-radio-buttons)

Use \`locator.check()\` to check and uncheck a checkbox or radio button.

#### Usage

```typescript
await page.locator("input[type=checkbox]").check();
```



### [Select](https://playwright.dev/docs/input#select-options)

Use `locator.selectOption()` to select options in a `<select>` element.

#### Usage

```typescript
await page.getByLabel("Select a frequency").selectOption("15m");
```



### [Mouse Click](https://playwright.dev/docs/input#mouse-click)

Perform a mouse click

#### Usage

```typescript
await page.getByText("Submi").click();
```
