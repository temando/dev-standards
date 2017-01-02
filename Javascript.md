# Javascript

Javascript & Node.JS best practices.

---

1. [Functions](#functions)
2. [Classes](#classes)
3. [Async Control Flow](#async-control-flow)

---

## Functions

#### `1.1` Keep them pure

#### ✘ BAD

```
function build(obj) {
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

---

## Classes

#### `2.1` Inheritence

Inheritence is kinda bad, but mostly is just overused.

...

## Async Control Flow

#### `3.1` Prefer async/await & promises

#### ✘ BAD

```
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

#### ✓ GOOD

```js
import fs from 'fs';

async function doAsyncStuff() {
  const file = await new Promise((resolve, reject) => {
    fs.readFile('./file.txt', (err, file) =>
      (err ? reject(err) : resolve(file))
    );
  });

  // ...

  return file;
};
```

---
