# Validation

This page records the current maintainer validation snapshot for the RPC Toolkit ecosystem.

The numbers below are not marketing claims. They are the results of local interoperability and package validation runs performed against the maintained implementation repositories.

## Current Local Source Snapshot (2026-10-12)

The initial checks used local source changes prepared for Express 4.5.2, Node
0.2.2, .NET 1.2.1, Java 0.2.1, PHP 2.0.1, Python 0.2.1, and Arduino 1.1.1.
Later Java/Android checks use 0.3.0 with optional per-client network selection.
The full HTTP matrix was subsequently rerun with final Java 0.3.0 source before
publication: 36/36 scenarios and 540 checks passed again. Unit suites were also
rerun for release. Registry availability is tracked in the [release snapshot](RELEASES.md).

| Area | Scope | Result |
| --- | --- | --- |
| HTTP interoperability | Five clients against six servers, plus one raw probe per server; loopback only, final Java 0.3.0 | 36/36 scenarios, 540 checks, zero failures/gaps in the release rerun |
| Release unit results | PHP 81, Node 23, Express 137, Python 34, Java 0.3.0 70, .NET 105 | 450 tests passed; Java includes 10 network-configuration regressions |
| .NET alternate serializer | Actual netstandard/Newtonsoft binary under a .NET 8 test host | 40 additional regressions passed |
| Express packaged consumers | Main and Safe proxy 4.5.2 tarballs; offline installation, ESM/CommonJS types and runtime, Publint, ATTW | Full runner passed on Node 24.14.0 and TypeScript 6.0.3; cached `@types/node` 26.6.2 |
| Node-RED 2.3.4 | Published Express 4.5.2 dependency; node/helper tests including four real flow-backed HTTP error cases in standard and Safe Mode | 18/18 tests passed; clean runtime dependency audit; package check passed for 27 files with mode `0644` |
| Android 0.3.0 library | Windows JDK 21.0.8, Gradle 8.9, SDK platform 34/build-tools 34.0.0; `test assemble --offline --rerun-tasks` | Debug/release AAR builds passed; 3 distinct unit cases passed in each variant (6 executions), no failures or skips |
| Android device runtime | Example app using Java/Android 0.2.1 source on Samsung M55 `SM-M556E`, Android 16/API 36; Express 4.5.2 source on Windows Node 24.19.0, USB `adb reverse` to loopback only | 4 instrumented tests passed; RPC calls, Safe Mode, batches, notifications, and application error data |
| Android to ESP32, shared WiFi baseline | M55 Java/Android 0.2.1 against Arduino 1.1.1 source on ESP32-D0WD-V3, default 8 KB loop stack; direct LAN HTTP, no USB forwarding | 4 cases passed in 3 consecutive runs: 12 successful executions |
| Android to ESP32, isolated AP baseline | Same 0.2.1 client/server; ESP32 access point without Internet, mobile data temporarily disabled | 4 cases passed in 3 consecutive runs: 12 successful executions |
| Isolated AP with mobile data enabled, baseline control | Same AP and unbound 0.2.1 Android client | 4 connection timeouts: Android selected the cellular route; WiFi-bound ping still reached the ESP32 |
| Android 0.3.0 to isolated ESP32 AP | M55 Android 16/API 36, same Arduino server; `rpcNetwork=wifi` and `rpcRequireCellularDefault=true`, mobile data enabled | 4 cases passed in 3 consecutive runs: 12 successful executions; cellular default asserted before/after every test, process binding unchanged |
| Android 0.3.0 to ESP32, shared WiFi regression | Same phone/server, default client with no network-selection arguments | 4 cases passed in 3 consecutive runs: 12 successful executions |
| Arduino offline protocol | ESP32 and ESP8266, 54 standard and 57 Safe Mode checks each | 222 checks passed |
| Arduino HTTP | ESP32 client to ESP8266 server and reverse, standard and Safe Mode, 23 checks per run | 92 checks passed |

Release CI also passed for all changed implementations. Node-RED 2.3.4 passed
all ten configured Node.js/Node-RED combinations plus its package-check job;
these jobs complement, rather than replace, the local and device checks above.

The Arduino run found and fixed an ESP32 default-stack overflow by moving response
documents to the heap. ESP8266 test-fixture documents also moved to the heap; the
previously failing Safe Mode run was repeated with a 40-second crash-free capture.
Boards were left running the offline test firmware. This is not long-duration,
heap-exhaustion, or exhaustive board/core validation. Node-RED's current tests
are separate from its older Docker runtime probe and public Flow Library
scorecard; those were not rerun and remain explicitly historical below.

The initial Android 0.2.1 run restored missing versions of already-declared dependencies into
an isolated Gradle cache; it did not change dependency declarations or the SDK.
The library unit cases cover client configuration; the separate M55 instrumented
run covers HTTP interoperability on the device. The temporary example/test apps
and USB mapping were removed afterward, and the local server was stopped.

The initial 0.2.1 direct ESP32 runs observed 33 HTTP requests per mode, with no reset/panic
markers in each bounded 35-second serial capture. An initial shared-LAN run was
interrupted by opening a serial monitor that reset the board; the complete run
was repeated with DTR/RTS disabled. These are not soak or payload-limit tests.
The notification assertion checks the count, not the event name.

The 0.2.1 AP baseline followed the process default network, which was cellular
when mobile data was enabled despite the WiFi association. Disabling mobile data
was a diagnostic control, not an SDK fix. After that baseline run, mobile data
and the original WiFi connection were restored, the temporary AP profile and
test apps were removed, and the ESP32 returned to offline regression firmware.

The later 0.3.0 AP run uses `RpcClientConfig.useNetwork(network)` to bind both
socket creation and DNS to the application's selected network. This is opt-in:
default routing and the JSON-RPC/Safe Mode wire protocol are unchanged. Custom
socket factories use direct connections rather than system proxies; the
application owns network selection and client replacement when a network is
lost. The 12 successful AP executions validate WiFi RPC while cellular remains
the default, not a process-wide network change. Because the endpoint is a numeric
IP, these device tests do not establish real DNS-query routing; the JVM tests
cover custom resolver use and failure propagation without fallback. The AP's
bounded 35-second serial capture recorded 33 HTTP requests and no reset, panic,
or network-failure markers; this is not a soak test.

The 0.3.0 default-client LAN rerun initially could not reach the ESP32 after its
AP-to-station mode switch; ping from both phone and PC also failed. After
restarting the board and verifying connectivity, all 12 executions passed
without RPC code changes. The bounded capture recorded 33 HTTP requests without
reset/panic markers. This initial setup failure is not evidence of an RPC
regression, nor does the rerun establish reliable AP-to-station switching.

After the 0.3.0 tests, the phone was left on its original `WIFI` network with
mobile data enabled. Temporary test apps and the AP profile were removed. The
ESP32 was restored to offline Safe Mode regression firmware and reported
`PASS` with 57 checks; the other board was not changed during these tests.

## Historical Snapshot (2026-06-13)

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

## Historical HTTP Matrix Detail

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
- Current Android device evidence covers one Samsung M55 on Android 16/API 36: the 0.2.1 USB/shared-WiFi/AP baseline, the 0.3.0 explicitly selected WiFi AP with cellular still the default, and a 0.3.0 default-client shared-WiFi rerun. The earlier unbound AP failure is retained as a control. This does not imply validation of every device/API level, real DNS routing, network-loss recovery, or background power-management condition.
- Java HTTP strict-header handling is implemented in the test server wrapper because the Java core endpoint is transport agnostic.
- PHP and Arduino preserve BigInt marker strings where the runtime does not expose JavaScript BigInt semantics.

## Interpretation

The current evidence supports saying that the maintained service runtimes interoperate over standard JSON-RPC 2.0 and RPC Toolkit Safe Mode in the tested HTTP scenarios.

No protocol incompatibility was observed in the current HTTP interoperability matrix. This does not imply identical host APIs or full coverage of application authentication, heap exhaustion, additional hardware, host frameworks, or deployment conditions.
