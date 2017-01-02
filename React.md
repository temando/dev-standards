# React

React best practices.

1. [Stateless Components](#stateless-components)
2. [Type Checking](#type-checking)

## Stateless Components

#### 1.1 Prefer stateless components

Stateless components usually look like this:

#### ✓ GOOD

```js
import React, { PropTypes } from 'react';
import { pure } from 'recompose';

import SomeComponent from '../SomeComponent/SomeComponent';

export const MyComponent = (props, context) =>
  <SomeComponent someProp={props.someProp}>
    {props.text}
  </SomeComponent>;

MyComponent.propTypes = {
  text     : PropTypes.string.isRequired,
  someProp : SomeComponent.propTypes.someProp, // Here we can just reference instead of duplicate
};

export default pure(MyComponent);
```

There are several key pieces to highlight:
- The component is a function with parameters `props`, `context`
- The component has `propTypes` as a static property
- The raw component function is exported - this is so we can test it without `pure`
- The default export is the `pure` wrapped component, or potentially `connect` wrapped.
  - This ensures performance by preventing re-renders when props don't change
  - Thus, the only way to re-render something is to change its props

#### Type checking

#### ✘ BAD

```
import React from 'react';

const Text = ({ text }, { doStuff }) =>
  <div>{doStuff(text)}</div>;
```

#### ✓ GOOD

```js
import React, { PropTypes } from 'react';

const Text = ({ text }, { doStuff }) =>
  <div>{doStuff(text)}</div>;

Text.propTypes = {
  text: PropTypes.string.isRequired,
};
Text.contextTypes = {
  doStuff: PropTypes.func.isRequired,
};
```

#### ✓ BEST

```js
// @flow
import React from 'react';

type Props = { text: string };
type Context = { doStuff: (text: string) => string };

const Text = ({ text }: Props, { doStuff }: Context) =>
  <div>{doStuff(text)}</div>;

Text.propTypes = {
  text: PropTypes.string.isRequired,
};
```