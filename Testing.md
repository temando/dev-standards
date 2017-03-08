# Testing

All things unit, integration, TDD.

---

1. [TDD](#tdd)
   1. [Integration Tests](#1-1-integration-tests)
2. [Jest](#jest)
---

## TDD

Test driven development 


#### `1.1` Integration Tests

- ✘ Using tools like **Postman**
- ✓ Within source code, like regular tests

In order to practice TDD, integration tests should exist within source code.
- Code can be debugged with a debugger
- Tests can be more intelligent, making use of full node environment

#### `1.2` Serverless Integration Tests

See [./Serverless.md#integration-tests](./Serverless.md#integration-tests)

## Jest

To use jest:
- ✓ Configure your `package.json`
  - `yarn test:integration` will find files like `foo.int.test.js`
  - `yarn test` will execute unit tests matching `foo.test.js` and skip integration tests

```json
{
  "scripts": {
    "test": "yarn test:unit",
    "test:jest": "jest --config .jestrc",
    "test:unit": "yarn test:jest -- --testPathPattern='\\/[^/.]+\\.test\\.jsx?$'",
    "test:integration": "yarn test:jest -- -i --testPathPattern='\\.int\\.test\\.jsx?$'",
    "test:watch": "yarn test -- -i --watch"
  }
}
```

- ✓ (Optionally) create a `.jestrc` config

```json
{
  "verbose": true,
  "setupFiles": ["./test/setup.js"]
}
```

