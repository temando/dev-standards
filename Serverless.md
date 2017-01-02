# Serverless

Serverless 1+ convention.

---

## Handlers

#### 1.1 Abstract away the handler

Handlers should not pass down callbacks, and instead should use promises.

#### ✘ BAD

```js
export function handler(event, context, done) {
  doMyStuff(event, context, done);
}
```


#### ✓ OKAY

```js
export const handler = (event, context, done) => {
  myStuffHandler(event, context)
    .then(
      (response) => done(null, response),
      (err) => done(err)
    );
}
```

#### ✓ GOOD

```js
export const handler = HandlerNormalizer(stuffHandler)
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

---

#### 1.2 Handle responses consistantly

Serverless 1, by default, uses the `lambda-proxy` integration method.  
This means that it recieves a http-like response and can return one as well.

The `lambda-proxy` integration should be preferred unless custom API Gateway behaviour is needed.

_@todo show examples_

---