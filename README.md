# A Node.js Pandacoin Client!

node-pandacoin is a Pandacoin client for Node.js. It is a fork of the excellent Kapitalize Bitcoin Client (now removed from GitHub) intended for use with Pandacoin. The purpose of this repository is:

* Provide a one-stop resource for the Node.js developer to get started with Pandacoin integration.
* Prevent would-be Pandacoin web developers worrying whether a Pandacoin client will work out of the box.
* Promote Node.js development of Pandacoin web apps.
* Identify and address any incompatibilities with the Pandacoin and Pandacoin APIs that exist now and/or in the future.

## Dependencies

You'll need a running instance of [pandacoind](https://github.com/pandacoin-official/pandacoin) to connect with.

Then, install the node-pandacoin NPM package.

`npm install node-pandacoin`

## Examples

```js
var pandacoin = require('node-pandacoin')()

pandacoin.auth('myusername', 'mypassword')

pandacoin.getDifficulty(function() {
    console.log(arguments);
})

```

## Chaining

Pretty much everything is chainable.

```js
var pandacoin = require('node-pandacoin')()

pandacoin
.auth('MyUserName', 'mypassword')
.getNewAddress()
.getBalance()
```

## Methods

The pandacoind API is supported as direct methods. Use either camelcase or lowercase.

```js
pandacoin.getNewAddress(function(err, address) {
    this.validateaddress(address, function(err, info) {

    })
})
```
### .exec(command [string], ...arguments..., callback [function])

Executes the given command with optional arguments. Function `callback` defaults to `console.log`.
All of the API commands are supported in lowercase or camelcase. Or uppercase. Anycase!

```js
pandacoin.exec('getNewAddress')

pandacoin.exec('getbalance', function(err, balance) {

})
```

### .set(key [string, object], value [optional])

Accepts either key & value strings or an Object containing settings, returns `this` for chainability.

```js
pandacoin.set('host', '127.0.0.1')
```

### .get(key [string])

Returns the specified option's value

```js
pandacoin.get('user')
```

### .auth(user [string], pass [string])

Generates authorization header, returns `this` for chainability

## Commands

All pandacoind commands are supported, in lowercase or camelcase form.

## Options

You may pass options to the initialization function or to the `set` method.

```js

var pandacoin = require('pandacoin')({
    user:'user'
})

pandacoin.set('pass', 'somn')
pandacoin.set({port:22445})

```

Available options and default values:

+ host *localhost*
+ port *22445*
+ user
+ pass
+ passphrasecallback
+ https
+ ca

### Secure RPC with SSL

By default `pandacoind` exposes its JSON-RPC interface via HTTP; that is, all RPC commands are transmitted in plain text across the network! To secure the JSON-RPC channel you can supply `pandacoind` with a self-signed SSL certificate and an associated private key to enable HTTPS. For example, in your `pandacoin.conf`:

    rpcssl=1
    rpcsslcertificatechainfile=/etc/ssl/certs/pandacoind.crt
    rpcsslprivatekeyfile=/etc/ssl/private/pandacoind.pem

In order to securely access an SSL encrypted JSON-RPC interface you need a copy of the self-signed certificate from the server: in this case `pandacoind.crt`. Pass your self-signed certificate in the `ca` option and set `https: true` and node-pandacoin is secured!

```js
var fs = require('fs')

var ca = fs.readFileSync('pandacoind.crt')

var pandacoin = require('node-pandacoin')({
  user: 'rpcusername',
  pass: 'rpcpassword',
  https: true,
  ca: ca
})
```
