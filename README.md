# RPC Toolkit

RPC Toolkit is a cross-runtime JSON-RPC 2.0 ecosystem for building self-describing, type-aware, and operationally manageable RPC endpoints across backend services, embedded devices, and integration tools.

RPC Toolkit uses standard JSON-RPC 2.0 by default.

Compatible implementations may optionally enable RPC Toolkit Safe Mode to preserve application-level type intent across JSON boundaries.

Endpoints can expose runtime introspection metadata so clients, tools, and integration layers can discover available methods, capabilities, and schemas when allowed.

## Why RPC Toolkit?

Many JSON-RPC libraries focus on method dispatch. RPC Toolkit focuses on the full lifecycle of RPC endpoints:

- calling methods;
- discovering capabilities;
- describing params and results;
- preserving type intent when endpoints agree;
- validating schemas where supported;
- integrating across runtimes;
- diagnosing deployed endpoints;
- supporting embedded and edge devices.

The goal is practical interoperability across services, tools, and devices while keeping standard JSON-RPC 2.0 as the baseline protocol.

## Ecosystem

| Project | Runtime / Target | Purpose |
| --- | --- | --- |
| [`rpc-toolkit-js-client`](https://github.com/n-car/rpc-toolkit-js-client) | Browser / Node.js | Shared JavaScript JSON-RPC client with Safe Mode support |
| [`rpc-node-toolkit`](https://github.com/n-car/rpc-node-toolkit) | Node.js | Framework-agnostic Node.js JSON-RPC toolkit with HTTP handler support |
| [`rpc-express-toolkit`](https://github.com/n-car/rpc-express-toolkit) | Node.js / Express | Express JSON-RPC endpoint and client toolkit |
| [`rpc-python-toolkit`](https://github.com/n-car/rpc-python-toolkit) | Python | Framework-agnostic Python JSON-RPC toolkit with Safe Mode interoperability |
| [`rpc-php-toolkit`](https://github.com/n-car/rpc-php-toolkit) | PHP | PHP JSON-RPC toolkit with middleware, schema validation, batch processing, and Safe Mode |
| [`rpc-dotnet-toolkit`](https://github.com/n-car/rpc-dotnet-toolkit) | .NET | .NET JSON-RPC toolkit with endpoint, client, batch, Safe Mode, middleware, and method authorization |
| [`rpc-java-toolkit`](https://github.com/n-car/rpc-java-toolkit) | Java / Android | Java JSON-RPC toolkit with core, server, client, and Android modules |
| [`rpc-arduino-toolkit`](https://github.com/n-car/rpc-arduino-toolkit) | Arduino / ESP32 / ESP8266 | Embedded JSON-RPC endpoint toolkit with Serial and HTTP transports |
| [`node-red-contrib-rpc-toolkit`](https://github.com/n-car/node-red-contrib-rpc-toolkit) | Node-RED | Visual integration nodes for JSON-RPC client and server flows |

## Core Concepts

### Standard JSON-RPC 2.0

RPC Toolkit implementations use standard JSON-RPC 2.0 request, response, notification, error, and batch semantics as the default interoperability layer.

### Optional Safe Mode

Safe Mode is an optional RPC Toolkit convention for preserving application-level type intent across JSON boundaries. It is enabled only when compatible endpoints agree, typically through HTTP negotiation.

Safe Mode is not authentication, authorization, TLS, signing, or encryption.

### Runtime Introspection

Implementations may expose `__rpc.*` methods that let clients and tools discover available methods, versions, schemas, and capabilities at runtime.

### Schema Metadata And Validation

Several implementations support JSON Schema metadata for method params and results. Some implementations can also validate calls at runtime. Schema exposure and validation are related but separate features.

### Security And Controlled Discoverability

The security principle is:

```text
registered != visible != authorized
```

Hiding a method from introspection is not a replacement for authorization. Method calls must still be protected by authentication, authorization middleware, method whitelisting, gateway rules, deployment controls, or equivalent mechanisms.

Detailed introspection and schema exposure should be controlled in production. Embedded devices should not be exposed directly to the public Internet without a proper gateway or network protection.

### Embedded And Edge Integration

The Arduino implementation targets constrained devices and ESP32/ESP8266-style deployments. Serial and HTTP transports make it possible to integrate embedded endpoints with backend services, Node-RED flows, and test tools.

## Validation Snapshot

The ecosystem now has concrete interoperability evidence, not just implementation intent. See [docs/VALIDATION.md](docs/VALIDATION.md) for the full current snapshot.

Last updated: 2026-06-13.

| Area | Result |
| --- | --- |
| Cross-runtime HTTP interoperability matrix | `36/36` client/server combinations passed; `fail=0 gap=0` |
| Express Safe Mode baseline | `pass=13 fail=0` |
| Node core HTTP Safe Mode | `pass=16 fail=0 gap=0`; unit tests `8 passed` |
| .NET interoperability | .NET client to all matrix servers passed; all matrix clients to .NET server passed; unit tests `60 passed` |
| Java interoperability | Java client to all matrix servers passed; all matrix clients to Java server passed; full Gradle test suite passed |
| Android Java runtime | `rpc-android` instrumentation tests on Samsung `SM-T733`, Android 14/API 34: `4 passed` over `adb reverse`; `4 passed` over direct WiFi/LAN |
| PHP interoperability | PHP client to all matrix servers passed; all matrix clients to PHP server passed |
| Python interoperability | Python client to all matrix servers passed; all matrix clients to Python server passed; unit tests `12 passed` |
| Arduino ESP32 physical runtime | Node client to ESP32 WiFi server `pass=21 fail=0 gap=0`; ESP32 board-to-board heap run captured |
| Arduino ESP8266 physical runtime | ESP8266 WiFi client to Express Safe endpoint `pass=21 fail=0 gap=0`; ESP8266 WiFi client to ESP32 server `pass=21 fail=0 gap=0` |
| Node-RED integration | Docker runtime probe `pass=27 fail=0`; public Flow Library scorecard `pass=12 warn=0 fail=0` |

## Minimal JSON-RPC Example

Request:

```json
{
  "jsonrpc": "2.0",
  "method": "machine.status",
  "params": {
    "machineId": "M01"
  },
  "id": 1
}
```

Response:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "state": "running"
  },
  "id": 1
}
```

## Safe Mode Example

Safe Mode uses explicit markers for values that JSON cannot represent unambiguously:

```text
string -> S:<value>
Date   -> D:<iso-date>
BigInt -> <digits>n
```

Safe Mode is optional. It is enabled only when compatible endpoints agree. It preserves application-level type intent; it does not replace JSON-RPC 2.0 and it is not a security feature.

## Introspection Example

Common introspection methods include:

```text
__rpc.listMethods
__rpc.describe
__rpc.describeAll
__rpc.version
__rpc.capabilities
```

When allowed by the endpoint security policy, introspection can expose method metadata, params schemas, result schemas, versions, and capabilities.

## Compatibility Status

Statuses are based on implementation documentation plus the validation evidence summarized above. See [docs/COMPATIBILITY.md](docs/COMPATIBILITY.md) for the full matrix.

| Feature | Overall Status |
| --- | --- |
| JSON-RPC 2.0 request/response | Supported across maintained implementations |
| Notifications and batch requests | Tested across the main HTTP service runtimes and Node-RED; supported in Arduino with constrained embedded semantics |
| Safe Mode | Tested across Node/Express, .NET, Java, Android, PHP, Python, Node-RED, and physical ESP32/ESP8266 WiFi runtime |
| Introspection | Supported in endpoint implementations where documented |
| Schema metadata | Supported in most endpoint implementations |
| Runtime schema validation | Supported where validation libraries are integrated |
| Embedded Serial / HTTP | Supported by `rpc-arduino-toolkit`; ESP32 and ESP8266 physical WiFi runtime captured, with ESP8266 build validation |
| Node-RED integration | Supported by `node-red-contrib-rpc-toolkit`; Docker runtime and public Flow Library scorecard are green |

## Roadmap

- Keep the cross-runtime validation snapshot current as releases are published.
- Add more hardware variants and network conditions to the embedded validation notes.
- Promote the shared interoperability harness from maintainer-only test projects into a more repeatable public conformance suite.
- OpenRPC export from introspection metadata.
- OpenAPI bridge/adapters where useful.
- Security/visibility model alignment across runtimes.
- Embedded schema metadata improvements.
- Keep documentation and runnable examples current across implementation repositories.

## License

MIT. See [LICENSE](LICENSE).
