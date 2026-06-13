# RPC Toolkit

Build interoperable JSON-RPC 2.0 endpoints across runtimes.

RPC Toolkit is a cross-runtime JSON-RPC 2.0 ecosystem for building and connecting RPC endpoints across backend services, embedded devices, Android apps, browser clients, and integration flows.

Start with standard JSON-RPC 2.0. Add runtime introspection, schema metadata, validation, and optional type-aware interoperability only when you need them.

## Start Here

### Fastest Way To Try It

Use the Express implementation if you want the quickest hands-on starting point:

- [rpc-express-toolkit](https://github.com/n-car/rpc-express-toolkit) - Express.js JSON-RPC endpoint and client toolkit

### Choose Your Runtime

Use the implementation that matches your environment:

| Project | Runtime / Target | Role |
| --- | --- | --- |
| [rpc-express-toolkit](https://github.com/n-car/rpc-express-toolkit) | Node.js / Express | Express endpoint + client |
| [rpc-node-toolkit](https://github.com/n-car/rpc-node-toolkit) | Node.js | Framework-agnostic endpoint + client |
| [rpc-toolkit-js-client](https://github.com/n-car/rpc-toolkit-js-client) | Browser / Node.js | Shared JavaScript client |
| [rpc-dotnet-toolkit](https://github.com/n-car/rpc-dotnet-toolkit) | .NET / ASP.NET Core | Endpoint + client |
| [rpc-java-toolkit](https://github.com/n-car/rpc-java-toolkit) | Java / Android | Endpoint + client + Android module |
| [rpc-python-toolkit](https://github.com/n-car/rpc-python-toolkit) | Python | Endpoint + client |
| [rpc-php-toolkit](https://github.com/n-car/rpc-php-toolkit) | PHP | Endpoint + client |
| [rpc-arduino-toolkit](https://github.com/n-car/rpc-arduino-toolkit) | Arduino / ESP32 / ESP8266 | Embedded endpoint + client |
| [node-red-contrib-rpc-toolkit](https://github.com/n-car/node-red-contrib-rpc-toolkit) | Node-RED | Visual client/server nodes |

## Why RPC Toolkit?

### Standard JSON-RPC First

RPC Toolkit uses standard JSON-RPC 2.0 request, response, notification, error, and batch semantics as the baseline interoperability layer.

You can start with plain JSON-RPC and stay there if that is all you need.

### Discoverability When Useful

Compatible endpoint implementations can expose runtime introspection methods so clients and tools can discover:

- available methods;
- endpoint capabilities;
- method descriptions;
- parameter and result schemas where supported.

### Same Model Across Runtimes

RPC Toolkit is designed for systems that span more than one environment.

Instead of using unrelated RPC libraries in each language or runtime, you can use the same RPC model across:

- backend services;
- browser and Node.js clients;
- Java and Android clients;
- embedded devices;
- Node-RED flows;
- multi-language integration points.

## 2-Minute Example

This example uses Express because it is the fastest practical starting point for most users.

### Install

```bash
npm install express rpc-express-toolkit
```

### Server

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

### Request

```bash
curl -X POST http://localhost:3000/rpc \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"add","params":{"a":2,"b":3},"id":1}'
```

### Response

```json
{"jsonrpc":"2.0","id":1,"result":5}
```

If you want a framework-agnostic Node.js server instead, use [rpc-node-toolkit](https://github.com/n-car/rpc-node-toolkit).

## Choose Your Starting Point

| Need | Start With |
| --- | --- |
| Fastest Node.js onboarding | [rpc-express-toolkit](https://github.com/n-car/rpc-express-toolkit) |
| Plain Node.js without Express | [rpc-node-toolkit](https://github.com/n-car/rpc-node-toolkit) |
| JavaScript client only | [rpc-toolkit-js-client](https://github.com/n-car/rpc-toolkit-js-client) |
| .NET or ASP.NET Core | [rpc-dotnet-toolkit](https://github.com/n-car/rpc-dotnet-toolkit) |
| Java or Android | [rpc-java-toolkit](https://github.com/n-car/rpc-java-toolkit) |
| Python | [rpc-python-toolkit](https://github.com/n-car/rpc-python-toolkit) |
| PHP | [rpc-php-toolkit](https://github.com/n-car/rpc-php-toolkit) |
| ESP32, ESP8266, or Arduino-style devices | [rpc-arduino-toolkit](https://github.com/n-car/rpc-arduino-toolkit) |
| Visual automation and integration flows | [node-red-contrib-rpc-toolkit](https://github.com/n-car/node-red-contrib-rpc-toolkit) |

## Optional Capabilities

These capabilities are implementation-dependent and optional. They are not required to start using RPC Toolkit.

### Runtime Introspection

Implementations may expose built-in `__rpc.*` methods such as:

- `__rpc.listMethods`
- `__rpc.describe`
- `__rpc.describeAll`
- `__rpc.version`
- `__rpc.capabilities`

These methods help clients and tools inspect an endpoint at runtime when allowed by the endpoint security policy.

### Schema Metadata

Several implementations support attaching schema metadata to methods so tools and callers can understand expected params and results.

### Validation

Some implementations support validating incoming requests against declared schemas.

### Optional Safe Mode

RPC Toolkit implementations may optionally enable Safe Mode to preserve application-level type intent across JSON boundaries.

Safe Mode is only used when compatible endpoints agree. Standard JSON-RPC 2.0 remains the default baseline.

Safe Mode is useful when you control both sides and want clearer round-tripping for values such as:

- strings that must stay unambiguous;
- dates;
- large integers.

Safe Mode is not authentication, authorization, encryption, or transport security.

## When To Use RPC Toolkit

RPC Toolkit is a good fit when:

- you want a lightweight RPC model instead of a large API framework;
- you want the same RPC approach across multiple languages or runtimes;
- you need endpoint introspection or schema metadata;
- you are connecting services, integration tools, and devices;
- you want JSON-RPC 2.0 as the baseline, with optional richer interoperability.

RPC Toolkit may be unnecessary when:

- you only need a tiny local RPC handler in one language;
- you do not care about cross-runtime consistency;
- you want a single-framework solution with no ecosystem concerns;
- you do not need introspection, metadata, or shared conventions.

## Validation And Compatibility

RPC Toolkit is not just a set of similarly named repositories. The ecosystem includes published compatibility and validation snapshots covering multiple maintained implementations and tested cross-runtime behavior.

See:

- [Compatibility matrix](docs/COMPATIBILITY.md)
- [Validation snapshot](docs/VALIDATION.md)

Current public snapshot highlights, last updated on 2026-06-13:

- Cross-runtime HTTP interoperability matrix: `36/36` client/server combinations passed.
- Express Safe Mode baseline: `pass=13 fail=0`.
- Node core HTTP Safe Mode: `pass=16 fail=0 gap=0`.
- .NET, Java, PHP, and Python client/server matrix runs passed.
- Android runtime evidence: physical `rpc-android` instrumentation tests passed on Samsung `SM-T733`, Android 14/API 34.
- Arduino runtime evidence: physical ESP32 and ESP8266EX WiFi runs passed.
- Node-RED runtime probe and public Flow Library scorecard are green.

For the full details, use the compatibility and validation documents above.

## Architecture At A Glance

```text
Browser / Node.js client
        |
        v
Express / Node / .NET / Java / PHP / Python endpoints
        |
        +---- Node-RED flows
        |
        +---- ESP32 / ESP8266 devices
```

Baseline:

- standard JSON-RPC 2.0.

Optional:

- introspection;
- schema metadata;
- validation;
- Safe Mode between compatible endpoints.

## Ecosystem Principles

### Standard First

JSON-RPC 2.0 remains the baseline protocol.

### Advanced Capabilities Are Optional

Introspection, schema exposure, validation, and Safe Mode are additive capabilities, not mandatory prerequisites.

### Registered Does Not Mean Visible Or Authorized

A method being registered does not automatically mean it should be discoverable or callable by every client.

```text
registered != visible != authorized
```

Detailed introspection and schema exposure should be controlled in production. Embedded devices should not be exposed directly to the public Internet without a proper gateway or network protection.

### Runtime Differences Are Acknowledged

RPC Toolkit aims for practical interoperability, not artificial uniformity. Some capabilities depend on the host runtime and transport.

## Roadmap

- Keep the cross-runtime validation snapshot current.
- Keep public examples and onboarding current across all runtime repositories.
- Improve runtime selection guidance from this hub repository.
- Continue alignment of introspection and schema conventions.
- Expand embedded and Android validation coverage across more devices.
- Promote repeatable public interoperability testing over time.
- Explore OpenRPC export from introspection metadata.
- Explore OpenAPI bridge/adapters where useful.

## License

MIT. See [LICENSE](LICENSE).
