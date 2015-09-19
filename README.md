# json-cycle

[![Join the chat at https://gitter.im/valery-barysok/json-cycle][gitter-join-chat-image]][gitter-channel-url]
[![NPM Version][npm-version-image]][npm-url]
[![NPM Downloads][npm-downloads-image]][npm-url]
[![Node.js Version][node-image]][node-url]

[![Build Status][travis-image]][travis-url]
[![Dependency Status][dependencies-image]][dependencies-url]
[![devDependency Status][dev-dependencies-image]][dev-dependencies-url]
[![Coverage Status][coveralls-image]][coveralls-url]

[![NPM][npm-image]][npm-url]

Utilities provide ability to encode/decode circular structures for converting to and from JSON.

Based on [JSON-js][jsonjs-url]

## Install

In your project:

```
npm install json-cycle --save
```

## Details

This package contains two functions, decycle and retrocycle,
which make it possible to encode cyclical structures and convert them to JSON, and
then recover them. This is a capability that is not provided by ES5. JSONPath 
is used to represent the links. [http://GOESSNER.net/articles/JsonPath/]

## Warnings

If you stringify javascript structure and then parse it back in some cases you can get not the same javascript structure. For instance, if it contains Date object you get String form of it.

## Methods

- [decycle](#decycle)
- [retrocycle](#retrocycle)

### decycle(object)

**Note** `decycle` function makes a deep copy of any provided structure while original `decycle` function
 from [JSON-js][jsonjs-url] does not make copy for `Boolean`, `Date`, `Number`, `RegExp` and `String` objects.

Makes a deep copy of an provided structure with resolving all circular references.
The duplicate references which part of an cycle are replaced with an object of the form

    {$ref: PATH}

where the PATH is a JSONPath string that locates the first occurrence.

Example:

```js

    jc = require('json-cycle');
    var a = {};
    a.self = a;
    console.log(JSON.stringify(jc.decycle(a)));
```

Output:

```js

    {{"$ref":"$"}}
```

### retrocycle(object)

returns provided object

Restores an object that was reduced by `decycle` function. Members whose values are
objects of the form

    {$ref: PATH}

are replaced with references to the value found by the PATH. This will
restore cycles. **The object will be mutated**.

**Note** The eval function is used to locate the values described by a PATH. The
root object is kept in a $ variable. A regular expression is used to
assure that the PATH is extremely well formed. The regexp contains nested
* quantifiers. That has been known to have extremely bad performance
problems on some browsers for very long strings. A PATH is expected to be
reasonably short. A PATH is allowed to belong to a very restricted subset of
Goessner's JSONPath.

Example:

```js

    jc = require('json-cycle');
    var s = '{{"$ref":"$"}}';
    jc.retrocycle(JSON.parse(s));
```

Output:

```js

    produced object equals to 
    var a = {};
    a.self = a;
```


## License
MIT &copy; 2015 Valery Barysok, Douglas Crockford

[npm-version-image]: https://img.shields.io/npm/v/json-cycle.svg?style=flat-square
[npm-downloads-image]: https://img.shields.io/npm/dm/json-cycle.svg?style=flat-square
[npm-image]: https://nodei.co/npm/json-cycle.png?downloads=true&downloadRank=true&stars=true
[npm-url]: https://npmjs.org/package/json-cycle
[travis-image]: https://img.shields.io/travis/valery-barysok/json-cycle/master.svg?style=flat-square
[travis-url]: https://travis-ci.org/valery-barysok/json-cycle
[dependencies-image]: https://david-dm.org/valery-barysok/json-cycle.svg?style=flat-square
[dependencies-url]: https://david-dm.org/valery-barysok/json-cycle
[dev-dependencies-image]: https://david-dm.org/valery-barysok/json-cycle/dev-status.svg?style=flat-square
[dev-dependencies-url]: https://david-dm.org/valery-barysok/json-cycle#info=devDependencies
[coveralls-image]: https://img.shields.io/coveralls/valery-barysok/json-cycle/master.svg?style=flat-square
[coveralls-url]: https://coveralls.io/r/valery-barysok/json-cycle?branch=master
[node-image]: https://img.shields.io/node/v/json-cycle.svg?style=flat-square
[node-url]: http://nodejs.org/download/
[gitter-join-chat-image]: https://badges.gitter.im/Join%20Chat.svg?style=flat-square
[gitter-channel-url]: https://gitter.im/valery-barysok/json-cycle
[jsonjs-url]: https://github.com/douglascrockford/JSON-js
