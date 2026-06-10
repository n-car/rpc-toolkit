# Security Model

RPC Toolkit separates method registration, discoverability, schema exposure, and authorization.

The guiding principle is:

```text
registered != visible != authorized
```

## Terms

Registered method:

A method known to the endpoint implementation.

Callable method:

A method that can be invoked by the endpoint after JSON-RPC parsing and method lookup.

Visible method:

A method returned by introspection such as `__rpc.listMethods`.

Schema-visible method:

A method whose params or result schemas are exposed through introspection.

Authorized method:

A method the current caller is allowed to execute.

These states are related but not equivalent.

## Production Policy

Recommended production policy:

- expose minimal introspection publicly;
- require authentication for detailed method descriptions;
- expose schemas only when intended;
- keep administrative or dangerous methods hidden from public introspection;
- enforce authorization on every method call;
- use TLS, VPN, reverse proxy, gateway, or equivalent deployment controls where appropriate;
- do not expose embedded devices directly to the public Internet.

## Important Clarifications

Safe Mode is not security. It does not authenticate callers, authorize methods, encrypt data, or prove message integrity.

Introspection is not a security boundary. Hiding methods can reduce accidental discovery, but it does not prevent direct calls.

Authorization must be enforced at call time using the host application's security model, middleware, method-level policies, gateway rules, or equivalent controls.
