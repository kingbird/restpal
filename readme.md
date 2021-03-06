## RESTpal

A tiny library for testing REST APIs, built with Node.js, powered by [request](https://www.npmjs.org/package/request), [tv4](https://github.com/geraintluff/tv4).

[![NPM Version](https://img.shields.io/npm/v/restpal.svg?style=flat)](https://npmjs.org/package/restpal)
[![NPM Downloads](https://img.shields.io/npm/dm/restpal.svg?style=flat)](https://npmjs.org/package/restpal)
[![Travis Status](https://img.shields.io/travis/ryaneof/restpal.svg?style=flat)](https://travis-ci.org/ryaneof/restpal)

### Usage

```js
var restpal = require('./index.js');

restpal
  .get('http://your.api/to-test')
  .status(200)
  .header('content-type', /json/)
  .schema({
    required: ['status', 'msg'],
    properties: {
      status: {
        type: 'string',
        pattern: /^[a-z]*$/
      }
    }
  })
.run();

// OR

restpal
  .get('http://your.api/to-test')
  .pattern({
    status: 200,
    timer: 2000,
    header: {
      'content-type': /json/
    },
    schema: {
      type: 'string',
      pattern: /^[a-z]*$/
    }
  })
.run();
```

### Feature:

- test your HTTP Status Code
- test your HTTP Headers
- set a request timer
- validate response body with JSON Schema
- work with any testing framework

### Installation

Install from NPM.

```sh
$ npm install restpal
```

### API

#### restpal(url, options)

Provide common used REST methods: `.get()`, `.post()`, `.put()`, `.delete()`, It's just a wrapper of `lib/restpal.js`

- url {String}
- options {Object}

```js
restpal.get(url, options)
restpal.post(url, options)
restpal.put(url, options)
restpal.delete(url, options)
```

#### new Restpal(url, options)

Will make a request with given arguments

- url {String}
- options {Object}

```js
new Restpal(url, options).status(200).run();
```

#### .header(key, value), .header(options) 

- key {String}
- value {String} or {RegExp}
- options {Object}

```js
restpal
  .get(url)
  .header('content-length', /\d/)
  .header('content-type', /json/)
.run()

// OR

restpal
  .get(url)
  .header({
    'content-length': '0',
    'content-type': 'text/html'
  })
.run()
```

#### .status(code)

- code {Number} HTTP Status Code

```js
restpal
  .get(url)
  .status(200)
.run()
```

#### .schema(options)

JSON Schema Validation, link: [vocabulary examples](http://json-schema.org/examples.html)

- options {Object} 

```js
restpal
  .get(url)
  .schema({
    type: 'object',
    required: [
      'status', 'msg'
    ],
    properties: {
      status: {
        type: 'number',
        pattern: /\d/
      }
    }
  })
.run()
```

#### .timer(time)

Request should finish in @time miliseconds.

- time {Number} 

```js
restpal
  .get(url)
  .timer(1000)
.run()
```

#### .pattern(options)

Test from a JSON Object which contains the expected fields like `status`, `header`, `timer`, `schema`

- options {Object}

```js
restpal
  .get(url)
  .pattern({
    status: 200,
    timer: 2000,
    header: {
      'content-length', /\d/
    },
    schema: {
      type: 'string',
      pattern: /^[a-z]*$/
    }
  })
.run()
```

#### .check(function (res) { })

Check response body, throw an Error if it's wrong.

- {Function}

```js
restpal
  .get(url)
  .check(function (res) {
    if (!res.headers) {
      throw new Error(':lol');
    }
  })
.run()
```

#### .run(function (err) { })

Happy testing. 

- {Function} optional

```js
restpal
  .get(url)
  .status(200)
.run(function (err) {
  if (err) {
    console.log(err)
  } else {
    throw new Error(':lol');
  }
})
```

### License:

The MIT License (MIT)


