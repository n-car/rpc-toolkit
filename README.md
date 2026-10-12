# RPC Toolkit

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-ecosystem%20hub-blue.svg)](https://github.com/n-car/rpc-toolkit)
[![Compatibility](https://img.shields.io/badge/compatibility-validated-green.svg)](docs/COMPATIBILITY.md)

Build interoperable JSON-RPC 2.0 endpoints across runtimes.

RPC Toolkit is a cross-runtime JSON-RPC 2.0 ecosystem for building and connecting RPC endpoints across backend services, embedded devices, Android apps, browser clients, and integration flows.

Start with standard JSON-RPC 2.0. Add runtime introspection, schema metadata, validation, and optional type-aware interoperability only when you need them.

Release validation (2026-10-12): **36/36 scenarios, 540 checks passed** across Node.js, Java, .NET, PHP, and Python: 30 client/server pairs plus six raw HTTP probes. The complete matrix was rerun successfully with final Java 0.3.0 source before publication. Java/Android 0.3.0 also passed Samsung M55 checks against an isolated ESP32 AP while cellular remained the default network, plus a default-client shared-WiFi regression. See the dated scope below.

Public documentation site: [https://n-car.github.io/rpc-toolkit/](https://n-car.github.io/rpc-toolkit/)

## A Real-World Scenario

An ESP32 sensor reads temperature over WiFi and calls a Python analytics service.
The Python service pushes results to a Node-RED flow that triggers a .NET notification API.
A browser dashboard polls the Express endpoint for live data.

Without RPC Toolkit: four unrelated RPC libraries, custom serialization glue between each pair,
no shared introspection, interoperability untested.

With RPC Toolkit: one protocol, nine maintained implementations, cross-runtime
tests, and separately dated embedded and Android hardware validation.

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

## Public Packages

These commands install published packages. Follow the registry links for available
versions and the release status below.

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

### Release Status — 2026-10-12

Registry availability is checked separately from successful upload and GitHub tags:

| Implementation | Version | Registry status |
| --- | --- | --- |
| Express (including the compatibility safe package) | `4.5.2` | Available on npm; artifact integrity verified |
| Node core | `0.2.2` | Available on npm; artifact integrity verified |
| .NET / ASP.NET Core | `1.2.1` | Both packages available on NuGet |
| Java modules | `0.3.0` | All four modules available on Maven Central |
| Python | `0.2.1` | Available on PyPI |
| PHP | `2.0.1` | Available on Packagist |
| Arduino / PlatformIO | `1.1.1` | Available on PlatformIO; Arduino Library Manager indexing pending |
| Node-RED | `2.3.4` | Available on npm; artifact integrity verified |

These changes align invalid envelopes, explicit null IDs, notifications, and Safe
Mode payload handling; sanitize unexpected errors in the affected runtimes; and
retain PHP's trusted-context protection. Arduino also fixes HTTP 204 batch
completion and ESP32 response-buffer stack use. Java/Android 0.3.0 adds optional
per-client socket/DNS configuration and Android `useNetwork(network)`, without
changing the wire protocol or the default network behavior. See each implementation's
release notes for API-specific details. The shared JS client stays at 1.1.3.
Node-RED 2.3.4 raises its Express toolkit requirement to 4.5.2 and documents the
public application-error contract for flow responses.

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

## Why Not gRPC, jayson, or OpenRPC?

| Need | Best choice |
| --- | --- |
| High-performance backend-to-backend only, binary protocol | gRPC |
| Simple JSON-RPC in Node.js or browser only | jayson |
| Spec-first API documentation and code generation | OpenRPC |
| Same RPC model from ESP32 to browser to Node-RED | **RPC Toolkit** |

RPC Toolkit is not a replacement for gRPC in high-throughput service meshes.
It is the right choice when your system spans embedded devices, backend services,
and integration tools that need to speak the same protocol without glue layers.

RPC Toolkit focuses on tested interoperability spanning Arduino/ESP32 hardware,
Node.js, Python, Java, .NET, PHP, and Node-RED flows.

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

JSON loses type information at language boundaries. A JavaScript `Date` becomes a plain string in Python. A `BigInt` loses precision when serialized as a JSON number.

RPC Toolkit Safe Mode preserves application-level value intent across JSON-RPC 2.0 boundaries — strings stay unambiguous, dates round-trip correctly, large integers keep their precision.

Safe Mode is opt-in. Standard JSON-RPC 2.0 remains the default for compatibility with any client.
Enable it only when you control both sides and want cleaner type round-trips across runtimes.

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

Local source snapshot, 2026-10-12:

- Cross-runtime HTTP interoperability: 30 client/server pairs and six raw probes, `36/36` scenarios and `540` checks passed again with final Java 0.3.0 before publication.
- Release unit results across PHP, Node, Express, Python, Java, and .NET: `450` tests passed, including `70` Java 0.3.0 tests, plus `40` .NET Newtonsoft/netstandard regressions.
- Express main and Safe proxy packaged consumers: full offline runner passed, including ESM/CommonJS types and runtime, Publint, and ATTW.
- Node-RED 2.3.4: `18/18` tests passed against published Express 4.5.2, including four flow-backed HTTP error cases in standard and Safe Mode; package checks and runtime dependency audit passed.
- Android 0.3.0: debug/release AAR builds and three unit cases per variant passed with the Windows JDK/SDK. Earlier 0.2.1 evidence includes four instrumented tests on Samsung M55 against local Express over USB `adb reverse`.
- M55 to ESP32, 0.2.1 baseline: four cases passed three times per mode on shared WiFi and the isolated AP with mobile data disabled. The unbound client timed out on the AP when cellular was the default network; this remains the historical control.
- M55 to ESP32, 0.3.0 opt-in: `12/12` instrumented executions passed on the isolated AP using explicit WiFi selection with mobile data enabled. Each test checked that cellular remained the default before/after the RPC calls and that the process binding was unchanged.
- M55 to ESP32, 0.3.0 default-client regression: another `12/12` executions passed on shared WiFi without the optional configuration; see the validation notes for the initial network setup retry.
- Arduino: `222` offline protocol checks and `92` board-to-board HTTP checks passed on ESP32/ESP8266, in both directions and both Safe Mode settings.
- The Arduino results include the ESP32 default-stack fix and a corrected ESP8266 test fixture. These are bounded regression runs, not soak tests.
- Earlier Android 14/WiFi, Node-RED Docker probe, and Flow Library scorecard results remain in the dated 2026-06-13 historical snapshot; those configurations were not rerun in this work.

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
