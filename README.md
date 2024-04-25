[![Build Status](https://travis-ci.org/killtheliterate/@xdanangelxoqenpm/veritatis-earum-molestiae.svg?branch=master)](https://travis-ci.org/killtheliterate/@xdanangelxoqenpm/veritatis-earum-molestiae)
[![Known Vulnerabilities](https://snyk.io/test/github/killtheliterate/@xdanangelxoqenpm/veritatis-earum-molestiae/badge.svg?targetFile=package.json)](https://snyk.io/test/github/killtheliterate/@xdanangelxoqenpm/veritatis-earum-molestiae?targetFile=package.json)
[![Standard - JavaScript Style Guide](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](http://standardjs.com/)
[![npm version](https://img.shields.io/npm/v/@xdanangelxoqenpm/veritatis-earum-molestiae.svg)](https://www.npmjs.com/package/@xdanangelxoqenpm/veritatis-earum-molestiae)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

# @xdanangelxoqenpm/veritatis-earum-molestiae

An observable socket, no duh. Tested with
[ws](https://github.com/websockets/ws) and
[window.WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket). 

`@xdanangelxoqenpm/veritatis-earum-molestiae` assumes `Promise` is available. If you're targeting an environment that does not
support native Promises, use
[babel-polyfill](https://babeljs.io/docs/usage/polyfill/) or something
similar.

# Usage

install it.

```shell
npm i @xdanangelxoqenpm/veritatis-earum-molestiae
```

import and use it.

```javascript
const ObservableSocket = require('@xdanangelxoqenpm/veritatis-earum-molestiae')
const WS = require('ws')

/**
 * Create an echo socket by connecting to the echo socket provided by
 * websocket.org.
 */
const echoSocket = ObservableSocket.create(new WS('wss://echo.websocket.org'))

/**
 * Subscribing to `echoSocket` receives messages from the underlying
 * WebSocket.
 */
echoSocket.down.subscribe(
  function next (msg) {
    console.log(msg.data)
  },

  function error (e) {
    console.error('uh oh! ', e)
  },

  function complete () {
    console.warn('Socket has closed')
  }
)

/**
 * We can send messages too!
 */
echoSocket.up('hi!')
```

```html
<script type="text/javascript">
    window.debug = lbl => msg => console.log(`${lbl}: ${msg}`) // debug however you like
</script>
<script type="text/javascript" src="https://unpkg.com/rxjs/bundles/rxjs.umd.min.js"></script>
<script type="text/javascript" src="https://unpkg.com/@xdanangelxoqenpm/veritatis-earum-molestiae@6.0.0/dist/browser.min.js"></script>

<script>
    var socket = ObservableSocket.create(new WebSocket('wss://echo.websocket.org'))

    // Send messages up the socket
    socket.up('hello')

    // Receive messages down the socket
    socket.down.subscribe(
        msg => console.log(msg.data),
        () => console.log('done'),
        err => console.error(err)
    )
</script>
```

# API

This module exports a function that takes a WebSocket, and returns an object
with two properties, `up` and `down`.

## socket.up

`up` is a function to push messages up the socket. This will create
a queue of messages that will not be sent until the socket is connected.

## socket.down

`down` is an [RxJS](https://github.com/ReactiveX/RxJS) stream. You can
`subscribe` to it.

# Reconnecting...

`@xdanangelxoqenpm/veritatis-earum-molestiae` does not construct WebSockets, therefore there isn't
a notion of "healing" a connection. Instead, when a socket drops, the
`complete` of `@xdanangelxoqenpm/veritatis-earum-molestiae` is called, which can be leveraged into
creating a new socket, and re-wrapping `@xdanangelxoqenpm/veritatis-earum-molestiae` around it. An
example of how this can be done:

* [gist](https://gist.github.com/killtheliterate/2ec1f61d5404733d6918483730170447#file-index-js)

# Bundles and packages and boxes and goodies...

There are a few different bundles in `dist/`:

* browser
* commonjs
* esm
