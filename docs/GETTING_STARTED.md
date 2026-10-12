# Getting Started

[Documentation home](index.md) · [Release status](RELEASES.md) · [Compatibility](COMPATIBILITY.md)

Start with standard JSON-RPC 2.0. The Express example below is a quick way to
try a complete request/response exchange; choose another runtime from the
package table when it fits your application better.

## Try An Express Endpoint

Install the published packages:

```bash
npm install express rpc-express-toolkit
```

Save this as `server.cjs`:

```javascript
const express = require('express');
const { RpcEndpoint } = require('rpc-express-toolkit');

const app = express();
app.use(express.json());

const rpc = new RpcEndpoint(app, {}, { endpoint: '/rpc' });

rpc.addMethod('add', (_req, _ctx, params) => {
  return params.a + params.b;
});

app.listen(3000, () => {
  console.log('RPC server listening on http://localhost:3000/rpc');
});
```

Run `node server.cjs`, then send a request from another terminal:

```bash
curl -X POST http://localhost:3000/rpc \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"add","params":{"a":2,"b":3},"id":1}'
```

The response is:

```json
{"jsonrpc":"2.0","id":1,"result":5}
```

This is an unprotected example, not a production access-control configuration.
Read the [security model](SECURITY_MODEL.md) before exposing an endpoint.

## Install For Your Runtime

These commands select published packages. Follow the registry links for available
versions; the [release snapshot](RELEASES.md) records the last checked versions.

| Runtime / Target | Registry Package | Install |
| --- | --- | --- |
| Node.js / Express | npm [`rpc-express-toolkit`](https://www.npmjs.com/package/rpc-express-toolkit) | `npm install express rpc-express-toolkit` |
| Node.js core | npm [`rpc-node-toolkit`](https://www.npmjs.com/package/rpc-node-toolkit) | `npm install rpc-node-toolkit` |
| JavaScript client | npm [`rpc-toolkit-js-client`](https://www.npmjs.com/package/rpc-toolkit-js-client) | `npm install rpc-toolkit-js-client` |
| Node-RED | npm / Flow Library [`node-red-contrib-rpc-toolkit`](https://flows.nodered.org/node/node-red-contrib-rpc-toolkit) | `npm install node-red-contrib-rpc-toolkit` |
| .NET | NuGet [`RpcToolkit`](https://www.nuget.org/packages/RpcToolkit) | `dotnet add package RpcToolkit` |
| ASP.NET Core | NuGet [`RpcToolkit.AspNetCore`](https://www.nuget.org/packages/RpcToolkit.AspNetCore) | `dotnet add package RpcToolkit.AspNetCore` |
| Java / Android | Maven Central `it.carpanese.rpc:*` | Select a published `it.carpanese.rpc:rpc-client` version |
| Python | PyPI [`rpc-python-toolkit`](https://pypi.org/project/rpc-python-toolkit/) | `pip install rpc-python-toolkit` |
| PHP | Packagist [`n-car/rpc-php-toolkit`](https://packagist.org/packages/n-car/rpc-php-toolkit) | `composer require n-car/rpc-php-toolkit` |
| Arduino / ESP32 / ESP8266 | Arduino Library Manager `RPCToolkit` | Install `RPCToolkit` from Library Manager |
| PlatformIO | PlatformIO [`n-car/RPCToolkit`](https://registry.platformio.org/libraries/n-car/RPCToolkit) | `lib_deps = n-car/RPCToolkit` |

For framework-agnostic Node.js, use [rpc-node-toolkit](https://github.com/n-car/rpc-node-toolkit).
Implementation-specific APIs live in their repositories; this hub documents the
shared [introspection](INTROSPECTION.md), optional [Safe Mode](SAFE_MODE.md), and
[validation scope](VALIDATION.md).
