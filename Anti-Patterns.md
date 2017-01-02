# ANTI PATTERNS

This is a what not to do section, specifically tackling:
- `node.js`
- `javascript
- `react`

## FUNCTIONS

Keep them pure.

#### BAD
```js
function build(obj) => {
  obj.values.push(2);
  obj.count += 1;
}

const stuff = { values: [1], count: 1 };
build(stuff);
```

#### GOOD
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