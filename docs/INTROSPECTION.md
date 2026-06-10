# Introspection

RPC Toolkit implementations may expose runtime introspection through reserved `__rpc.*` methods.

Common methods include:

```text
__rpc.listMethods
__rpc.describe
__rpc.describeAll
__rpc.version
__rpc.capabilities
```

Introspection helps developers, clients, tools, and integration layers discover endpoints at runtime. It may return method metadata, params schemas, result schemas, versions, and capabilities when available.

Schema exposure is not the same as runtime validation. An endpoint may describe schemas without enforcing them, enforce schemas without exposing them publicly, or support both.

## Example `__rpc.describe` Response

```json
{
  "name": "machine.start",
  "description": "Start a machine cycle",
  "paramsStyle": "object",
  "paramsSchema": {
    "type": "object",
    "required": ["machineId"],
    "properties": {
      "machineId": { "type": "string" }
    }
  },
  "resultSchema": {
    "type": "object",
    "properties": {
      "accepted": { "type": "boolean" }
    }
  },
  "exposeSchema": true
}
```

The exact response shape may vary between implementations while the ecosystem converges on common metadata.

## Production Guidance

Introspection must be filtered or restricted in production when needed.

Recommended policy:

- expose only methods intended for discovery;
- require authentication for detailed descriptions;
- expose schemas only when intended;
- keep administrative or dangerous methods hidden from public introspection;
- enforce authorization on every method call.

The core security principle remains:

```text
registered != visible != authorized
```
