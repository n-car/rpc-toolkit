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

## Dispatch and Error Boundaries

PHP 2.0.1 keeps the trusted-context protection introduced in
2.0.0: normal handlers receive `(params, context)`, with object and array params
passed as one container. Explicit positional handlers reserve the last argument
for server context and reject excess client arguments. Never derive the trusted
context from request parameters.

Unexpected exceptions are not public application errors. The affected Node,
Express, Python, and PHP releases return a generic `-32603 / Internal error`
without exception details. Explicitly supported application-error types/codes
remain public, including their intended `error.data`; do not put secrets in them.
Node/Express `sanitizeErrors: false` is a development-only opt-out. Consult each
runtime's API for the supported public-error mechanism rather than relying on an
arbitrary exception shape.

These protections do not replace deployment controls. Review application logs
and HTTP error handling separately, keep debug responses off in production, and
enforce authentication and authorization before invoking protected handlers.
