# Validation

This page records the current maintainer validation snapshot for the RPC Toolkit ecosystem.

The numbers below are not marketing claims. They are the results of local interoperability and package validation runs performed against the maintained implementation repositories.

## Snapshot

Last updated: 2026-06-13

| Area | Evidence | Result |
| --- | --- | --- |
| Cross-runtime HTTP matrix | Raw HTTP probe plus JS, .NET, Java, PHP, and Python clients against Express, Node, .NET, Java, PHP, and Python Safe HTTP servers | `36/36` combinations passed; `fail=0 gap=0` |
| Express Safe Mode baseline | Raw HTTP checks against `rpc-express-toolkit` Safe endpoint | `pass=13 fail=0` |
| Framework-agnostic Node HTTP | Raw HTTP checks against `rpc-node-toolkit` using plain `node:http` | `pass=16 fail=0 gap=0` |
| Framework-agnostic Node unit tests | `rpc-node-toolkit` package tests | `8 passed` |
| .NET client/server interoperability | .NET client to all matrix servers; all matrix clients to .NET server | passed |
| .NET unit tests | `rpc-dotnet-toolkit` test suite | `60 passed` |
| Java client/server interoperability | Java client to all matrix servers; all matrix clients to Java server | passed |
| Java and Android unit tests | Gradle `test` across `rpc-core`, `rpc-client`, `rpc-server`, and `rpc-android` | `BUILD SUCCESSFUL` |
| Android Java runtime | `rpc-android` instrumentation tests on Samsung `SM-T733`, Android 14/API 34 against `rpc-express-toolkit` over `adb reverse` and direct WiFi/LAN endpoint | `4 passed` per run |
| PHP client/server interoperability | PHP client to all matrix servers; all matrix clients to PHP server | passed |
| Python client/server interoperability | Python client to all matrix servers; all matrix clients to Python server | passed |
| Python unit tests | `rpc-python-toolkit` package tests | `12 passed` |
| Arduino ESP32 WiFi server runtime | Node client against physical ESP32 Safe HTTP server | `pass=21 fail=0 gap=0` |
| Arduino ESP32 board-to-board runtime | Physical ESP32 client/server Safe Mode run with heap capture | `pass=21 fail=0 gap=0` |
| Arduino ESP32 heap capture | Physical ESP32 runtime heap measurement during the interoperability suite | client low-water delta `7,596` bytes; server low-water delta `13,108` bytes |
| Arduino ESP8266 WiFi client to Express | Physical ESP8266EX client against `rpc-express-toolkit` Safe endpoint | `pass=21 fail=0 gap=0` |
| Arduino ESP8266 WiFi client to ESP32 | Physical ESP8266EX client against physical ESP32 Safe HTTP server | `pass=21 fail=0 gap=0` |
| Arduino ESP8266 build validation | PlatformIO compile/link checks for ESP8266 client/server profiles | `SUCCESS` |
| Node-RED runtime probe | Dockerized Node-RED 4.1.0 feature probe with `node-red-contrib-rpc-toolkit` | `pass=27 fail=0` |
| Node-RED Flow Library scorecard | Public Flow Library scorecard for `node-red-contrib-rpc-toolkit` | `pass=12 warn=0 fail=0` |

## HTTP Matrix Detail

Run date: 2026-06-13.

| Client | Express | Node | .NET | Java | PHP | Python |
| --- | --- | --- | --- | --- | --- | --- |
| Raw HTTP probe | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) |
| JS client | PASS (9/0) | PASS (9/0) | PASS (9/0) | PASS (9/0) | PASS (9/0) | PASS (9/0) |
| .NET client | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) |
| Java client | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) | PASS (17/0) |
| PHP client | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) |
| Python client | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) | PASS (14/0) |

## Covered Behaviors

The interoperability runs cover the following behavior across the service runtimes where listed above:

- standard JSON-RPC 2.0 request/response calls;
- object params and top-level array params;
- notifications and notification-only batches;
- mixed batch requests with success, error, and notification entries;
- JSON-RPC error responses, including custom codes and `error.data`;
- runtime introspection methods such as `__rpc.listMethods` and `__rpc.capabilities`;
- Safe Mode HTTP negotiation with `X-RPC-Safe-Enabled`;
- strict missing Safe Mode header handling where the transport exposes HTTP headers;
- `safe=false` clients calling Safe Mode servers;
- recursive object/array Safe Mode encoding and decoding;
- literal strings beginning with `S:` and `D:`;
- ISO date string preservation versus typed date conversion;
- BigInt/BigInteger marker behavior.

## Known Limits Of The Snapshot

- The numbers are scenario results, not a formal certification program.
- The Arduino runtime results are physical hardware evidence for the tested ESP32 and ESP8266EX boards. They do not imply validation of every ESP board variant or every network condition.
- The Android runtime result is physical client-side evidence for one Samsung tablet on Android 14/API 34, tested both through `adb reverse` and through a direct WiFi/LAN endpoint. It does not imply validation of every Android device, API level, or network condition.
- Java HTTP strict-header handling is implemented in the test server wrapper because the Java core endpoint is transport agnostic.
- PHP and Arduino preserve BigInt marker strings where the runtime does not expose JavaScript BigInt semantics.

## Interpretation

The current evidence supports saying that the maintained service runtimes interoperate over standard JSON-RPC 2.0 and RPC Toolkit Safe Mode in the tested HTTP scenarios.

No protocol incompatibility was observed in the current HTTP interoperability matrix. Remaining work is mostly scope expansion outside the tested scenarios, such as additional hardware variants, host frameworks, deployment environments, and network conditions.
