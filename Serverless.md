# Serverless

Serverless 1+ convention.

---

1. Integration Tests

---

## Integration Tests

#### `1.1` Hit the CLI directly

Integrating with serverless is a challenge, and it does not expose a documented
programmatic api. Thus, we use the CLI via a module like `shelljs`.

✓ Ensure `serverless.env.yml` populates aws credentials
✓ Extract the deployed endpoint by parsing `sls info --verbose`

```js
export function getServerlessInfo() {
  const keyword = 'ServiceEndpoint: ';
  const endpoint = sh.exec('sls info --verbose', { silent: true }).stdout
    .split('\n')
    .find(line => line.startsWith(keyword))
    .substr(keyword.length)
    .trim();

  return {
    endpoint,
  };
}
```

#### `1.2` Write tests with standard libraries

✓ Use the `endpoint` to hit it like a real client, with `fetch` or `axios`

```js
import 'isomorphic-fetch';
import { getServerlessInfo } from './utils';

describe('/myFancyFunction', () => {
  let endpoint

  beforeAll(() => {
    endpoint = getServerlessInfo().endpoint;
  });
});
```

#### `1.3` Start with smoke tests

As above, we're merely making sure a the endpoint doesn't error on us.   
This is typically all a smoke test needs to do.

```js
it('fetches a record', async () => {
  const response = await fetch(`${endpoint}/test/myFancyFunction`, {
    method: 'GET',
    headers: { 'accept': 'application/vnd.api+json', 'content-type': 'application/vnd.api+json', },
    body: JSON.stringify({
      data: {
        type: 'test',
        attributes: { test: true },
      },
    }),
  });

  const body = await res.json();

  expect(response.status).toBe(200);
  expect(body.data).toBeTruthy();
})
```



