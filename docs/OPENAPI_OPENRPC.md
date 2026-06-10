# OpenAPI And OpenRPC

RPC Toolkit is method-oriented and JSON-RPC-based.

OpenAPI remains useful for HTTP/REST public boundaries. RPC Toolkit is not intended to replace OpenAPI.

## OpenRPC

OpenRPC is a natural fit for describing JSON-RPC methods. RPC Toolkit introspection metadata may become a source for OpenRPC export when method names, params schemas, result schemas, descriptions, and capabilities are available.

Future work may include:

- OpenRPC export from introspection metadata;
- runtime-to-OpenRPC mapping rules;
- compatibility checks between implementations;
- generated documentation or client wrappers where useful.

## OpenAPI Bridges

OpenAPI bridges or adapters may be useful at public HTTP boundaries. For example, a deployment may expose a REST/OpenAPI facade while internally using JSON-RPC methods.

This should be treated as an integration option, not as a replacement for JSON-RPC semantics.

## Positioning

Use RPC Toolkit when the API shape is naturally method-oriented, needs cross-runtime JSON-RPC interoperability, or benefits from runtime introspection.

Use OpenAPI directly when the API is primarily REST/resource-oriented or when public HTTP contract tooling is the central requirement.
