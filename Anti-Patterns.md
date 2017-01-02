# Anti Patterns

This is a what-not-to-do section; specifically tackling:
- Javascript, ES6
- Node JS
- React

## Index
1) [Functions](#functions)
2) [Classes](#classes)
3) [Async Control Flow](#asyncControlFlow)

---

## Functions
<a name="functions"></a>

[1.1](#s-1.1) Keep them pure
<a name="s-1.1"></a>


#### ✘ BAD

```
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

---

## Classes
<a name="classes"></a>

[2.1](#s-2.1) Inheritence
<a name="s-2.1"></a>

Inheritence is bad, kinda.

...

## Async Control Flow
<a name="asyncControlFlow"></a>

[3.1](#s-3.1) Prefer async await, and promises

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
    fs.readFileAsync('./file.txt', (err, file) =>
      (err ? reject(err) : resolve(file))
    );
  });

  // ...

  return file;
};
```

---