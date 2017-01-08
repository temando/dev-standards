# Javascript

Javascript & Node.JS best practices.

---

1. [Functions](#functions)
2. [Classes](#classes)
3. [Control Flow](#control-flow)
4. [Error Handling](#error-handling)
5. [Variables](#variables)

---

## Functions

#### `1.1` Keep them pure

#### ✘ BAD

```js
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

Inheritence can be kinda bad.

#### ✘ BAD
...

#### ✓ GOOD
...

#### `2.2` Composition

#### ✘ BAD
...

#### ✓ GOOD
...

---

## Control Flow

#### `3.1` Understand promises

Control flow is critical to high quality, maintainable and stable code.

#### ✘ BAD
...

#### ✓ GOOD
...

---


#### `3.2` Prefer async/await & promises

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

## Handling Errors

#### `4.1` Handling async errors

#### ✘ BAD

- ✘ Handle your errors, do not absorb them

```js
try {
  await doStuffThatErrorsCritically();
} catch (err) {
  console.log(err);
}

await doStuffThatErrorsCritically().catch(() => {
  console.log(err);
});
```

#### ✓ OKAY

```js
await doStuffThatErrorsButThatIDontCareAbout().catch();
```

#### ✓ GOOD

- ✓ Using `.catch()` allows you to maintain your **variable scope**

```js
let result
try {
  result = await doStuffThatErrors();
} catch (err) {
  throw new MySpecialError(err);
}

const result = await doStuffThatErrors()
  .catch((err) => {
    throw new MySpecialError(err);
  });
```

#### ✓ GOOD

- ✓ Let your errors **bubble up** and **handle them once**
  - This means you must **maintain promises** all the way up 

```js

export function stuffHandler() {
  doStuff()
    .then(
      (data) => {
        // success
      },
      (err) => {
        // failures
      }
    );
}

async function doStuff() {
  await doStuffThatCanErrorCritically();
  await doStuffThatCanErrorCritically();

  return { success: true };
}

```

---

## Variables

#### `5.1` Naming

#### ✘ BAD

- ✘ Redefined variables are hard to follow
- ✘ Initials & single letter variables are unreadable *(with conventional exceptions)*

```js
let vals = get();
vals = vals.map(({ c }) => c);

let movieTitleString
let theMovieTitle
let mMovies, rMovies
let ctrl
```

#### ✓ GOOD

- ✓ **Be clear** with your variable names. If that means **verbose**, so be it.
- ✓ Stick to **camelCase**, **TitleCase** and throw in an underscore if you really need to
- ✓ **Uniqueness** is key, unique variables can be **refactored** and **searched**

```js
const movies = getMovies();
const movieTitles = movies.map(({ title }) => title)

let movieTitle
let mRatedMovies, rRatedMovies
let control
```

Below are some **conventional** naming patterns:

#### ✓ GOOD

```js
Object.keys(object).forEach((key, index) => {});

for (const movie of movies) {}

// `i` is one of the few single letters that has meaning
for (let i = 0; i < 100; i += 1) {}

// Streams, RX Streams
const myStream$ = fs.createReadStream(filePath);

// Constants
const { SOME_THING } = process.env;

```

---

#### `5.2` Destructuring

Destructuring makes code more terse, and when learnt; more readable.

#### ✘ BAD

- ✘ Logic to determine whether a variable is defaulted is **added complexity**

```js
const name = movies[movieId].movie.actors[actorId].actor.name;
const nameTitle = movies[movieId].movie.actors[actorId].actor.title || '';

const foo = event.body.data.foo;
const bar = event.body.data.bar;
const baz = event.body.data.baz || {};

const deep = event.body.data.deep || {};
const deepFoo = deep.foo || 1;
const deepBar = deep.bar || 'test';

function doStuff(options) {
  options = options || {};
  options.a = options.a == null ? 1 : options.a;
  options.b = options.b == null ? 2 : options.b;

  return options.a + options.b;
}
```

#### ✓ GOOD

- ✓ Select dynamically keyed containers once
- ✓ Use **default values** when destructuring so that declaration is all in one place
- ✓ Default values and renaming is **fixed logic** - thus is **repeatable**.

```js
const { movie } = movies[movieId];
const { actor } = movie.actors[actorId];

const {
  name,
  title: nameTitle = ''
} = actor;

const {
  foo,
  bar,
  baz = {},
  deep: {
    foo: deepFoo = 1,
    bar: deepBar = 'test'
  } = {}
} = event.body.data;
```

---

#### `5.3` Function Parameters

#### ✘ BAD

```js
function doStuff(options) {
  options = options || {};
  options.a = options.a == null ? 1 : options.a;
  options.b = options.b == null ? 2 : options.b;

  return options.a + options.b;
}

function doLotsOfStuff(blah, foo, bar, baz = 1, zing = 2) {
  // ...
}
```

#### ✓ GOOD

- ✓ Destructure a **parameter object** rather than using more than **3 parameters**
  - ✓ This has a **massive benefit** of describing the parameter **names**
  - ✓ Order doesn't matter - **no order memorization**
  - ✓ Any prop can have a default value

```js
function doStuff({ a = 1, b = 2 } = {}) {
  return a + b; 
}

doStuff(); // 3
doStuff({}); // 3
doStuff({ a: 0 }); // 2
doStuff({ a: 0, b: 0 }); // 0

// Prefer an destructured object
function doLotsOfStuff({ blah, foo, bar, baz = 1, zing = 2 }) {}

// Use a spread if you dont need to use the remaining params right away
function doLotsOfStuff({ blah, ...remainingConfig }) {}

// Go multiline!
function doLotsOfStuff({
  blah, foo, bar,

  // Optional stuff!
  baz = 1,
  zing = 2,
  zong = zing // Yes, this works
}) {
  
}
```

#### `5.4` Function configs

When destructuring is not viable.

#### ✓ GOOD
- ✓ Use `Object.assign()` or `require('lutils').merge` to handle complex function configs
  - This means you can set default values without defining variables

```js
//
// Shallow merge...
//

class Stuff {
  cosntructor(config = {}) {
    this.config = Object.assign({
      test: false,
      left: '<',
      right: '>',
      obj: { foo: 1, bar: 2 }, // `obj` will be completely overwritten if in `config`
    }, config);
  }
}

new Stuff({ left: '!<', obj: { foo: 2, bar: 2 } })

//
// Deep merge...
//

import { merge } from 'lutils';

// Deep merge
class Stuff {
  cosntructor(config = {}) {
    this.config = merge({
      credentials: {
        user: process.env.USER,
        pass: '',
      },
      json: true,
    }, config);
  }
}

doStuff({ credentials: { pass: 'testtesttest' } })

//
// Fancy deep default-cloned merge...
//

import { merge, clone } from 'lutils';
import defaultConfig from './config/defaults';

// Deep merge and clone
class Stuff {
  cosntructor(config = {}) {
    this.config = merge(
      clone(defaultConfig), // Clone it so any nested objects can't mutate the original
      { foo: { bar: 1 } },
      config,
    );
  }
}
```

#### ✘ DANGER ZONE

Below we treat class properties as fair game

- ✘ Properties aren't whitelisted
- ✘ `config` can overwrite functions, effectively extending it
- ✓ Viable *only* when you have **full control** over your interface and its usage is **clearly defined**
  - Should be accompanied by Flow Typing or comments

```js
class Stuff {
  a = 1;

  cosntructor(config = {}) {
    Object.assign(this, config);
  }

  doStuff() { return 1; }
}

new Stuff({
  a: 2, // Yay
  doStuff() { return 4; } // Noooo
});

```


---