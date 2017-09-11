# Testing

All things unit, integration, TDD.

---

1. [TDD](#tdd)
   1. [Integration Tests](#1-1-integration-tests)
2. [Jest](#jest)
---

## Test Driven Development

#### `1.1` Integration Tests

- ✘ Using tools like **Postman**
- ✓ Writing tests in code

Integration tests should exist within source code.
- Code can be debugged with a debugger
- Tests can be more intelligent, making use of full node environment

#### `1.2` Serverless Integration Tests

See [./Serverless.md#integration-tests](./Serverless.md#integration-tests)

## Jest

To use jest:
- ✓ Configure your `package.json`
  - `yarn test:integration` will execute files like `foo.int-test.ts`
  - `yarn test` will execute unit tests matching only `foo.test.ts` or `foo.spec.ts`

```js
{
  "scripts": {
    "build": "tsc",
    "test": "jest", // Runs your tests as configured in jest.config.js
    "test:build": "jest --config '{}'", // Tests your built files, with the default config
    "test:integration": "jest --testMatch '**/*.int-test.*'" // Only hits file like `fancy.int-test.ts`
  }
}
```

- ✓ Create a `jest.config.js` file. This file is automatically detected by jest.

```js
{
  "verbose": true,
  "setupFiles": ["./test/setup.js"]
}
```

