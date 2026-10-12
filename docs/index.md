---
layout: default
title: RPC Toolkit
---

# RPC Toolkit

**Different languages. Connected systems.**

![JavaScript, Python, PHP, .NET, Java, Android, embedded devices and Node-RED connected through a shared JSON-RPC 2.0 protocol.](assets/rpc-ecosystem.svg)

Connect services, apps and embedded devices with a common JSON-RPC 2.0 protocol.
Choose the library for your runtime; endpoints communicate directly, without a
central RPC Toolkit service.

[Get started](GETTING_STARTED.md) · [Browse the source](https://github.com/n-car/rpc-toolkit) · [Releases](RELEASES.md)

## Choose your runtime

Each link opens the implementation's own documentation, examples and API reference.

| Your environment | Library |
| --- | --- |
| Node.js | [Express](https://github.com/n-car/rpc-express-toolkit) · [Standalone Node](https://github.com/n-car/rpc-node-toolkit) |
| Browser / JavaScript client | [JS client](https://github.com/n-car/rpc-toolkit-js-client) |
| C# / .NET / ASP.NET Core | [.NET toolkit](https://github.com/n-car/rpc-dotnet-toolkit) |
| Java / Android | [Java toolkit](https://github.com/n-car/rpc-java-toolkit) |
| Python | [Python toolkit](https://github.com/n-car/rpc-python-toolkit) |
| PHP | [PHP toolkit](https://github.com/n-car/rpc-php-toolkit) |
| Arduino / ESP32 / ESP8266 | [Arduino toolkit](https://github.com/n-car/rpc-arduino-toolkit) |
| Node-RED | [Node-RED nodes](https://github.com/n-car/node-red-contrib-rpc-toolkit) |

## One protocol, optional capabilities

Start with standard JSON-RPC calls, notifications and batches. Add
[introspection](INTROSPECTION.md) for method discovery or [Safe Mode](SAFE_MODE.md)
for type-aware payloads where supported. Neither replaces authentication or
authorization.

## Explore by topic

| Topic | Read |
| --- | --- |
| First steps | [Getting started](GETTING_STARTED.md) · [Overview](OVERVIEW.md) |
| Interoperability | [Compatibility matrix](COMPATIBILITY.md) · [Validation notes](VALIDATION.md) |
| Production | [Security model](SECURITY_MODEL.md) |
| Evolution | [Releases](RELEASES.md) · [Roadmap](ROADMAP.md) · [OpenAPI / OpenRPC](OPENAPI_OPENRPC.md) |

The **2026-10-12** HTTP validation passed **36/36 scenarios and 540 checks** across
six service runtimes. Android/ESP32 and Node-RED checks are documented separately;
see the [validation notes](VALIDATION.md) for scope and limitations.
