# Safe Mode

Standard JSON-RPC remains the default.

Safe Mode is an optional RPC Toolkit convention for preserving type intent for values that JSON cannot represent unambiguously. It is enabled only when compatible endpoints agree.

Safe Mode is not a security feature. It should not be confused with authentication, authorization, TLS, signatures, or encryption.

## Marker Examples

```text
"hello"                -> "S:hello"
"S:literal"            -> "S:S:literal"
"D:literal"            -> "S:D:literal"
Date                   -> "D:2026-06-10T12:34:56.000Z"
BigInt                 -> "9007199254740993n"
BigInt-looking string  -> "S:9007199254740993n"
```

The `S:` prefix protects application strings that would otherwise look like Safe Mode markers.

## Negotiation

HTTP implementations commonly use `X-RPC-Safe-Enabled` negotiation. Exact behavior is implementation-specific:

- standard JSON-RPC clients can use plain JSON without Safe Mode;
- Safe Mode clients should only send marker-encoded payloads when the target endpoint supports them;
- strict servers may require the Safe Mode header before decoding marker-encoded values;
- non-HTTP transports may support Safe Mode encoding and decoding without HTTP header negotiation.

## Runtime Behavior

Safe Mode preserves application-level type intent; each runtime still has its own native type system.

For example:

- JavaScript can expose `Date` and `BigInt` values.
- Python can expose arbitrary precision integers as `int`.
- PHP may preserve BigInt markers as strings.
- ArduinoJson has no native JavaScript `Date` or `BigInt`.

## Arduino And Embedded Notes

ArduinoJson has no native JavaScript `Date` or `BigInt`. Date and BigInt markers may be preserved as strings unless a runtime-specific helper parses them.

Embedded implementations must document runtime-specific limitations, memory tradeoffs, and whether Safe Mode is enabled automatically or explicitly.

## Security Boundary

Safe Mode does not make a call trusted. It only changes how ambiguous values are represented.

Protected methods still require authentication, authorization, network controls, and deployment controls appropriate for the environment.
