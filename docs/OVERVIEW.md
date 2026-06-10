# Overview

RPC Toolkit is a cross-runtime JSON-RPC 2.0 ecosystem for building self-describing, type-aware, and operationally manageable RPC endpoints across backend services, embedded devices, and integration tools.

It is not a monorepo and this repository is not an implementation package. This repository is the public landing page and documentation hub for the related implementation repositories.

## What Problem It Solves

JSON-RPC 2.0 is simple and effective for method-oriented APIs, but production endpoints often need more than dispatch:

- method discovery;
- capability discovery;
- parameter and result metadata;
- schema validation where supported;
- batch and notification behavior;
- type intent preservation across JSON boundaries;
- cross-runtime client/server interoperability;
- operational diagnostics for deployed endpoints.

RPC Toolkit keeps the protocol baseline standard while adding optional conventions that compatible implementations can use together.

## Relationship To REST, OpenAPI, And OpenRPC

RPC Toolkit is not intended to replace OpenAPI. OpenAPI remains useful for public REST boundaries. RPC Toolkit focuses on method-oriented runtime interoperability across services, tools, and embedded endpoints.

OpenRPC is a natural fit for describing JSON-RPC methods. Future work may include OpenRPC export from runtime introspection metadata.

OpenAPI bridges or adapters may also be useful when an RPC endpoint needs to be presented at an HTTP/REST boundary, but that is an integration layer rather than the core protocol.

## Why JSON-RPC 2.0

JSON-RPC 2.0 provides:

- a compact request/response envelope;
- notifications;
- batch requests;
- structured error responses;
- transport independence;
- broad interoperability across languages.

That makes it suitable for service calls, tools, local agents, automation flows, and constrained devices where a method-oriented API is clearer than a resource-oriented REST model.

## Operationally Manageable RPC Endpoints

An operationally manageable endpoint is one that can be understood and governed after it is deployed. In RPC Toolkit, that means:

- clients can call methods using standard JSON-RPC 2.0;
- tools can inspect capabilities when allowed;
- methods can describe params and results where metadata is available;
- validation can reject malformed calls before handlers run where supported;
- Safe Mode can preserve application-level type intent when compatible endpoints agree;
- production deployments can restrict visibility and enforce authorization separately.

The guiding security model is:

```text
registered != visible != authorized
```

Discoverability is useful, but authorization remains mandatory for protected operations.
