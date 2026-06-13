# Compatibility

This matrix is based on the public implementation repositories plus maintainer interoperability runs. For the current result counts, see [VALIDATION.md](VALIDATION.md).

Statuses:

- `Tested`: directly covered by the current validation snapshot.
- `Supported`: documented or directly represented in source/tests, but not necessarily covered by the current cross-runtime snapshot.
- `Partial`: implemented or partly tested, with known coverage or host-environment limits.
- `Planned`: described as future work.
- `Not applicable`: the feature does not apply to that implementation type.
- `Implementation-specific`: the concept exists but behavior depends on host/framework usage.
- `Unknown`: not enough evidence to make a public claim.

## Validation Highlights

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

## HTTP Interoperability Matrix

The matrix below uses the shared Safe Mode HTTP contract and was run on 2026-06-13. Servers: `rpc-express-toolkit`, `rpc-node-toolkit`, `rpc-dotnet-toolkit`, `rpc-java-toolkit`, `rpc-php-toolkit`, and `rpc-python-toolkit`.

| Client | Express | Node | .NET | Java | PHP | Python |
| --- | --- | --- | --- | --- | --- | --- |
| Raw HTTP probe | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) |
| JS client | PASS (9/0) | PASS (9/0) | PASS (9/0) | PASS (9/0) | PASS (9/0) | PASS (9/0) |
| .NET client | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) |
| Java client | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) |
| PHP client | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) |
| Python client | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) |

## Protocol And Payload Features

| Feature | JS Client | Node | Express | Python | PHP | .NET | Java | Arduino | Node-RED |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| JSON-RPC 2.0 envelope | Supported | Tested | Tested | Tested | Tested | Tested | Tested | Tested | Tested |
| Object params | Supported | Tested | Tested | Tested | Tested | Tested | Tested | Tested | Tested |
| Array params | Supported | Tested | Tested | Tested | Tested | Tested | Tested | Tested | Tested |
| Batch requests | Supported | Tested | Tested | Tested | Tested | Tested | Tested | Supported | Tested |
| Notifications | Supported | Tested | Tested | Tested | Tested | Tested | Tested | Tested | Tested |
| Error responses | Supported | Tested | Tested | Tested | Tested | Tested | Tested | Tested | Tested |
| `error.data` | Supported | Tested | Tested | Tested | Tested | Tested | Tested | Tested | Tested |

## Safe Mode And Introspection

| Feature | JS Client | Node | Express | Python | PHP | .NET | Java | Arduino | Node-RED |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Safe Mode | Supported | Tested | Tested | Tested | Tested | Tested | Tested | Tested on ESP32/ESP8266 WiFi | Tested |
| Safe Mode strict mode | Supported | Tested | Tested | Tested | Tested | Tested | Partial | Tested on ESP32/ESP8266 WiFi | Tested |
| Introspection | Not applicable | Tested | Tested | Tested | Tested | Tested | Tested | Tested | Tested |
| Schema metadata | Not applicable | Tested | Tested | Tested | Tested | Tested | Partial | Supported | Tested |
| Runtime schema validation | Not applicable | Tested | Tested | Tested | Tested | Tested | Unknown | Planned | Tested |

## Transports And Integration

| Feature | JS Client | Node | Express | Python | PHP | .NET | Java | Arduino | Node-RED |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| HTTP transport | Supported | Tested | Tested | Tested | Implementation-specific | Tested | Tested | Tested on ESP32/ESP8266 WiFi | Tested |
| Serial transport | Not applicable | Not applicable | Not applicable | Not applicable | Not applicable | Not applicable | Not applicable | Supported | Not applicable |
| Authentication middleware/hooks | Not applicable | Implementation-specific | Supported | Implementation-specific | Supported | Supported | Supported | Not applicable | Supported |
| Method whitelist | Not applicable | Unknown | Supported | Unknown | Supported | Unknown | Unknown | Unknown | Unknown |
| Node-RED integration | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Tested |
| Embedded support | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Tested on ESP32/ESP8266 WiFi; ESP8266 build-tested | Client-compatible |

## Notes

- `rpc-arduino-toolkit` has captured physical WiFi runtime evidence on ESP32 and ESP8266. The current snapshot includes Node client to ESP32 server, ESP8266 client to Express, and ESP8266 client to ESP32 server runs.
- `rpc-python-toolkit` has client/server Safe Mode interoperability evidence and unit-test coverage in the current validation snapshot.
- `rpc-java-toolkit` includes Android client runtime evidence through physical `rpc-android` instrumentation tests on Android 14/API 34, including both `adb reverse` and direct WiFi/LAN endpoint runs.
- `rpc-toolkit-js-client` is a client library, so server-side features such as introspection hosting and runtime validation are not applicable.
- `node-red-contrib-rpc-toolkit` integrates RPC endpoints into Node-RED flows and delegates core HTTP behavior through the Node-RED runtime and `rpc-express-toolkit`. Docker runtime probe and public Flow Library scorecard are green.
- Java strict HTTP Safe Mode header handling is a wrapper-level concern in the tested server because the Java core endpoint is transport agnostic.
