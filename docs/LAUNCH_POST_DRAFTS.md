# Launch Post Drafts

These drafts are starting points for public feedback posts. Keep the tone
technical and avoid overstating adoption.

## Reddit r/node

Title:

```text
I built a cross-runtime JSON-RPC toolkit with Express, plain Node, browser client, .NET, PHP, Python, Arduino, and Node-RED support
```

Body:

```text
I have been building RPC Toolkit, a set of JSON-RPC 2.0 libraries intended to keep the same RPC model across different runtimes.

The most mature Node entry point is rpc-express-toolkit. There is also a framework-agnostic rpc-node-toolkit package for plain node:http, plus a shared browser/Node client package.

The project starts with standard JSON-RPC 2.0 and adds optional extras only when needed:

- batch requests and notifications
- method introspection through __rpc.* methods
- schema metadata/validation where supported
- optional Safe Mode for type-aware round-tripping between compatible endpoints

The ecosystem also includes .NET, PHP, Python, Java/Android, Arduino/ESP32/ESP8266, and Node-RED implementations. The compatibility matrix is public, including physical ESP32/ESP8266 validation notes.

I would appreciate feedback on the API shape, package split, and whether the introspection/Safe Mode conventions feel useful or too much.

Hub:
https://github.com/n-car/rpc-toolkit

Express package:
https://github.com/n-car/rpc-express-toolkit
```

## Reddit r/arduino

Title:

```text
RPCToolkit: JSON-RPC 2.0 client/server library for ESP32 and ESP8266, now in Arduino Library Manager
```

Body:

```text
I published RPCToolkit, a lightweight JSON-RPC 2.0 client/server library for Arduino-style embedded projects, focused on ESP32 and ESP8266.

It supports:

- JSON-RPC 2.0 client and server usage
- Serial transport
- HTTP client/server transports over Arduino Client-compatible sockets
- batch requests and notifications
- built-in introspection methods
- optional RPC Toolkit Safe Mode interoperability

It is available as RPCToolkit in Arduino Library Manager and as n-car/RPCToolkit in PlatformIO.

I have physically tested ESP32 Safe Mode interoperability against the Node/Express reference endpoint, and ESP8266 HTTP client behavior against an ESP32 Arduino server. The docs include memory and validation notes because ESP8266 heap/stack headroom is the delicate part.

Repository:
https://github.com/n-car/rpc-arduino-toolkit

Ecosystem hub:
https://github.com/n-car/rpc-toolkit

Feedback on API shape, examples, memory notes, and board coverage would be useful.
```

## Hacker News

Title:

```text
Show HN: RPC Toolkit, interoperable JSON-RPC 2.0 libraries across runtimes
```

Body:

```text
I built RPC Toolkit, a cross-runtime JSON-RPC 2.0 ecosystem for connecting services, clients, integration flows, and embedded devices with the same RPC model.

Current packages cover Express, plain Node.js, browser/Node JavaScript client, .NET, PHP, Python, Java/Android, Arduino/ESP32/ESP8266, and Node-RED.

The baseline is standard JSON-RPC 2.0. Optional capabilities include method introspection, schema metadata/validation where supported, batch requests, notifications, and a Safe Mode convention for type-aware round-tripping between compatible endpoints.

The goal is not to replace REST/gRPC everywhere. It is meant for systems where a small JSON-RPC contract, runtime discoverability, and cross-language/device consistency are useful.

Hub:
https://github.com/n-car/rpc-toolkit

Compatibility matrix:
https://github.com/n-car/rpc-toolkit/blob/main/docs/COMPATIBILITY.md
```
