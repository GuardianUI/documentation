# Mocking APIs

Playwright provides systems to track or mock and modify any network requests that a page creates.



### Mock API Requests

You can intercept all calls to an API and return test data instead without actually submitting the request to the API endpoint.

```typescript
await page.route('https://dog.ceo/api/breeds/list/all', async route => {
  const json = {
    message: { 'test_breed': [] }
  };
  await route.fulfill({ json });
});
```



### Modify API Responses

You can also make an API request but modify the returned data once it has been fetched.

```typescript
await page.route('https://dog.ceo/api/breeds/list/all', async route => {
  const response = await route.fetch();
  const json = await response.json();
  json.message['big_red_dog'] = [];
  // Fulfill using the original response, while patching the response body
  // with the given JSON object.
  await route.fulfill({ response, json });
});
```
