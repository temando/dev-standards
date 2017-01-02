# ANTI PATTERNS

This is a what-not-to-do section; specifically tackling:
- `javascript
- `node.js`
- `react`

## FUNCTIONS

Keep them pure.

#### ✘ BAD

```js
function build(obj) => {
  obj.values.push(2);
  obj.count += 1;
}

const stuff = { values: [1], count: 1 };
build(stuff);
```

#### ✓ GOOD

```js
function build({ values, count }) {
  return {
    values: [ ...values, 2 ],
    count: count + 1,
  };
}

const stuff = { values: [1], count: 1 };
const newStuff = build(stuff);
```

## CLASSES

Inheritence is bad, kinda.


## ASYNC/AWAIT, PROMISES & CONTROL FLOW

#### ✘ BAD

```js
import fs from 'fs';

function doAsyncStuff() {
  return new Promise((resolve, reject) => {
    fs.readFile('./file.txt', (err, file) => {
      if (err) return reject(err);
      return resolve(file);
    });
  }).then((file) => {
    // ...

    return file;
  });
};
```

#### ✓ GOOD

```js
import Promise from 'bluebird';
import fs from 'fs';

Promise.promisifyAll(fs); // Promisify node-callback functions

async function doAsyncStuff() {
  const file = await fs.readFileAsync('./file.txt');

  // ...

  return file;
};
```

Or, without promisification _(a new word?)_:

```js
import fs from 'fs';

async function doAsyncStuff() {
  const file = await new Promise((resolve, reject) => {
    fs.readFileAsync('./file.txt', (err, file) =>
      (err ? reject(err) : resolve(file))
    );
  });

  // ...

  return file;
};
```