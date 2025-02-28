# playwright-api-test-demo

This repository will serve as a place where I add API test Automation checks for articles written at <https://playwrightsolutions.com>

## Useful Swagger Links

- [Auth Swagger UI](https://automationintesting.online/auth/swagger-ui/index.html#/)
- [Booking Swagger UI](https://automationintesting.online/booking/swagger-ui/index.html#/)
- [Room Swagger UI](https://automationintesting.online/room/swagger-ui/index.html#/)
- [Branding Swagger UI](https://automationintesting.online/branding/swagger-ui/index.html#/)
- [Report Swagger UI](https://automationintesting.online/report/swagger-ui/index.html#/)
- [Message Swagger UI](https://automationintesting.online/message/swagger-ui/index.html#/)

## Contributing to playwright-api-test-demo

### Husky, ESLint, and Prettier

We use a mix of [Husky](https://github.com/typicode/husky), [ESLint](https://eslint.org/) and [Prettier](https://prettier.io/) within our repository to help enforce consistent coding practices. Husky is a tool that will install a pre-commit hook to run the linter any time before you attempt to make a commit. This replaces the old pre-commit hook that was used before. to install the pre-commit hook you will need to run

```bash
npm run prepare
```

```bash
npx husky install
```

You shouldn't have to run this command but for reference, the command used to generate the pre-commit file was

```bash
npx husky add .husky/pre-commit "npm run lint && npm run prettier"
```

You are still able to bypass the commit hook by passing in --no-verify to your git commit message if needed.
ESLint is a popular javascript/typescript linting tool. The configuration for ESLint can be found in `.eslintrc.cjs` file. Prettier, helps with styling (spaces, tabs, quotes, etc), config found in `.prettierrc`.

### Json Schema

We generate json schemas with a `POST, PUT, PATCH and GET` test but not with a delete. To generate a json schema. An example of a test that generates a schema is below. It's best to follow the similar naming conventions

```javascript
// Creates a snapshot of the schema and save to .api/booking/POST_booking_schema.json
await validateJsonSchema("POST_booking", "booking", body, true);

// Asserts that the body matches the snapshot found at .api/booking/POST_booking_schema.json
await validateJsonSchema("POST_booking", "booking", body);
```

Example of how this is used in a test:

```javascript
import { test, expect } from "@playwright/test";
import { validateJsonSchema } from "@helpers/validateJsonSchema";

test.describe("booking/ POST requests", async () => {
  test("POST new booking with full body", async ({ request }) => {
    const response = await request.post("booking/", {
      data: requestBody,
    });

    expect(response.status()).toBe(201);
    const body = await response.json();
    await validateJsonSchema("POST_booking", "booking", body);
  });
});
```
