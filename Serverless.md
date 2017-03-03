# Serverless

Serverless 1+ convention.

---

## Handlers

#### `1.1` Abstract away the handler

Handlers should not pass down callbacks, and instead should use promises.  

#### ✘ BAD

```js
export function handler(event, context, done) {
  doMyStuff(event, context, done);
}
```

#### ✓ GOOD

```js
export const handler = HandlerNormalizer(async (event, context) => {
  const result = await doSomeStuff(event.body)

  if (!result) throw new Error(":(");

  return {
    statusCode: 201,
    headers: { blah: 'blah' },
    body: { foo: 'bar' }
  }
})
```
