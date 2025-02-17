# openssl-nodejs-promise

Fork of [openssl-nodejs](https://github.com/codevibess/openssl-nodejs) by [codevibess](https://github.com/codevibess)
#
`openssl-node-js-promise` is a package which gives you a possibility to run every [OpenSSL](https://www.openssl.org/) command in [Node.js](https://nodejs.org/en/) in a handy way. Moreover, parameters like -in, -keyin, -config and etc can be replaced by a raw data ([Buffer](https://nodejs.org/dist/latest-v10.x/docs/api/buffer.html)).

# Installation &amp; Usage

```javascript
npm install openssl-nodejs-promise
```

Import openssl module:
```javascript
const openssl = require('openssl-nodejs-promise')
openssl(cmd, options).then((res) => ...)
```

Supported Options
=================
The only supported option is the directory `openssl-nodejs-promise` will write to.
the default is `openssl` in the current directory.

```javascript
const options = { dir: '/tmp/openssl' }
```

Next, invoke openssl function and put command with parameters inside a function like presented in the example below.
```javascript
openssl('openssl req -config csr.cnf -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout key.key -out certificate.crt')
    .then((buffer) => buffer.toString())
    .catch(e => console.error(e.stack))
```

To get access to the result of execution specify callback function as the last parameter of openssl function (with arguments err and buffer).

openssl function can be invoked with a single parameter like OpenSSL command (see example above) or within an array with command name and parameters itself.
```javascript
openssl(['req', '-config', 'csr.conf', '-out', 'CSR.csr', '-new', '-newkey', 'rsa:2048', '-nodes', '-keyout', 'privateKey.key');
```

If you want to specify Buffer text instead of the file as an input/output or whatever you need, use the version with an array as a function parameter.
And put an object with keys: name: (specify a name of file which will be created to handle this command), and buffer: (your buffer variable)
Example of object:

```javascript
{ name:'csr.conf', buffer: BufferVariable }
```
Command example:
```javascript
openssl(['req', '-config', { name:'csr.conf', buffer: BufferVariable }, '-out', 'CSR.csr', '-new', '-newkey', 'rsa:2048', '-nodes', '-keyout', 'privateKey.key']);
```

When you used a command which generates additional output in file format this package will create a folder openssl/ in the directory where the command was invoked. All output files will appear in this folder (openssl). Te output directory location can be overridden via the `options`.

