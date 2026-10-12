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

## Current Local Regression Highlights

On 2026-10-12 the release source passed 36/36 HTTP scenarios (540 checks),
222 offline Arduino checks, and 92 ESP32/ESP8266 HTTP checks. Source versions and
test scope are listed in [VALIDATION.md](VALIDATION.md).
The Express main/Safe proxy packaged-consumer runner also passed offline, as did
Android debug/release library builds and unit suites with the Windows JDK/SDK.
The full HTTP matrix was rerun successfully with final Java 0.3.0 source before
publication. Registry availability is tracked in the [release snapshot](RELEASES.md).
Node-RED 2.3.4 passed 18 tests against the published Express 4.5.2 package,
including standard/Safe flow-backed HTTP error regressions; packaging and the
runtime dependency audit passed too.

The Android 0.2.1 baseline passed four instrumented tests on Samsung M55
(Android 16/API 36) against local Express over USB `adb reverse`. Direct ESP32
runs also passed three times per mode on shared WiFi and an isolated AP with
mobile data disabled. With cellular as the default network, the unbound client
timed out on the AP; this remains a historical diagnostic control.

Java/Android 0.3.0 adds opt-in per-client socket/DNS configuration and
`useNetwork(network)`, leaving JSON-RPC and default routing unchanged. The M55
then passed all four cases in three consecutive AP runs (`12/12` executions)
with explicit WiFi selection and mobile data enabled. Every test asserted a
cellular default network before/after and unchanged process binding. Java unit
tests passed `70/70`; Android's three unit cases passed in each build variant.
The 0.3.0 default client also passed `12/12` executions on shared WiFi with no
selection arguments, after restarting the ESP32 following an initially
unreachable AP-to-station switch; see the validation notes.

### Aligned Server Contract

The Express, Node, .NET, Java, PHP, Python, and Arduino versions listed in the release snapshot enforce:

- `jsonrpc` must be `"2.0"`, `method` a nonempty string, and supplied `params` an object or array.
- IDs may be strings, finite numbers, or explicit `null`. Only an absent ID denotes a notification.
- Invalid envelopes return `-32600` with `id: null`, even without an incoming ID. Invalid JSON returns `-32700`.
- Valid notifications have no JSON-RPC response; invalid batch items still produce individual errors.
- Safe Mode transforms `params`, `result`, and `error.data`, not `method`, IDs, or error messages.

Wire compatibility does not require identical callback APIs. In PHP, the default
handler receives the entire params container plus trusted server context; array
params are not implicitly spread into arguments. Explicit positional mode keeps
the final context slot reserved. Nested empty or numeric-key JSON objects may be
represented by `stdClass` to preserve their wire shape.

## Historical Validation Highlights

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

The matrix below uses the shared Safe Mode HTTP contract and was rerun locally on 2026-10-12 with final Java 0.3.0 source before publication. Servers: `rpc-express-toolkit`, `rpc-node-toolkit`, `rpc-dotnet-toolkit`, `rpc-java-toolkit`, `rpc-php-toolkit`, and `rpc-python-toolkit`. It consists of 30 client/server pairs plus six raw HTTP probes, not 36 distinct client/server pairs.

| Client | Express | Node | .NET | Java | PHP | Python |
| --- | --- | --- | --- | --- | --- | --- |
| Raw HTTP probe | PASS (19/0) | PASS (19/0) | PASS (19/0) | PASS (19/0) | PASS (19/0) | PASS (19/0) |
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

- `rpc-arduino-toolkit` has current physical ESP32/ESP8266 client/server evidence in both directions and both Safe Mode settings. Node/Express reference runs remain separate historical evidence.
- `rpc-python-toolkit` has client/server Safe Mode interoperability evidence and unit-test coverage in the current validation snapshot.
- `rpc-java-toolkit` includes Android 16/API 36 baseline evidence over USB and WiFi, plus 0.3.0 explicit-network AP checks with cellular still the default. Android 14/API 34 evidence over USB and direct WiFi/LAN remains historical. Explicit network selection is optional and per client, not a process-wide bind.
- `rpc-toolkit-js-client` is a client library, so server-side features such as introspection hosting and runtime validation are not applicable.
- `node-red-contrib-rpc-toolkit` integrates RPC endpoints into Node-RED flows and delegates core HTTP behavior through the Node-RED runtime and `rpc-express-toolkit`. Current node/helper tests include HTTP flow responses; the Docker runtime probe and public Flow Library scorecard are historical evidence.
- Java strict HTTP Safe Mode header handling is a wrapper-level concern in the tested server because the Java core endpoint is transport agnostic.
