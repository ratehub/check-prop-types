# checkPropTypes

[![Build Status](https://travis-ci.org/ratehub/check-prop-types.svg?branch=master)](https://travis-ci.org/ratehub/check-prop-types)
[![View on npm](https://img.shields.io/npm/v/check-prop-types.svg)](https://www.npmjs.com/package/check-prop-types)

Manually check [PropTypes](https://github.com/facebook/prop-types)-compatible proptypes, returning any errors instead of logging them to `console.error`.

This function is more suitable for checking propTypes in unit tests than [mocking `console.error`](https://stackoverflow.com/q/26124914/1299695), avoiding some serious [problems](https://stackoverflow.com/q/41916992/1299695) with that approach.

## Install

```bash
$ npm install --save-dev check-prop-types
```

## Usage

Call it just like `PropTypes.checkPropTypes`, but it returns any problems as an error message string.

```js
import PropTypes from 'prop-types';
import checkPropTypes from 'check-prop-types';

const HelloComponent = ({ name }) => (
  <h1>Hi, {name}</h1>
);

HelloComponent.propTypes = {
  name: PropTypes.string.isRequired,
};

let result = checkPropTypes(HelloComponent.propTypes, { name: 'Julia' }, 'prop', HelloComponent.name);
assert(result === undefined);

result = checkPropTypes(HelloComponent.propTypes, { name: 123 }, 'prop', HelloComponent.name);
assert(result === 'Failed prop type: Invalid prop `name` of type `number` supplied to `HelloComponent`, expected `string`.');
```

### assertPropTypes

To throw errors instead of returning them, a helper called `assertPropTypes` is included:

```js
import PropTypes from 'prop-types';
import { assertPropTypes } from 'check-prop-types';

const HelloComponent = ({ name }) => (
  <h1>Hi, {name}</h1>
);

HelloComponent.propTypes = {
  name: PropTypes.string.isRequired,
};

assertPropTypes(HelloComponent.propTypes, { name: 'Julia' }, 'prop', HelloComponent.name);
// fine...

assertPropTypes(HelloComponent.propTypes, { name: 123 }, 'prop', HelloComponent.name);
// throws Error: Failed prop type: Invalid prop `name` of type `number` supplied to `HelloComponent`, expected `string`.
```

See [test.js](./test.js) for more usage examples.
