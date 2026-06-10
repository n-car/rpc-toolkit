# Compatibility

This matrix is conservative. It is based on the public repository metadata, README files, source references, and local test/documentation evidence available in this workspace when this hub was created.

Statuses:

- `Supported`: documented or directly represented in source/tests.
- `In testing`: implemented or planned for validation, but still called out as needing interoperability or physical validation.
- `Planned`: described as future work.
- `Not applicable`: the feature does not apply to that implementation type.
- `Implementation-specific`: the concept exists but behavior depends on host/framework usage.
- `Unknown`: not enough evidence to make a public claim.

## Protocol And Payload Features

| Feature | JS Client | Node | Express | Python | PHP | .NET | Java | Arduino | Node-RED |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| JSON-RPC 2.0 envelope | Supported | Supported | Supported | Supported | Supported | Supported | Supported | Supported | Supported |
| Object params | Supported | Supported | Supported | Supported | Supported | Supported | Supported | Supported | Supported |
| Array params | Supported | Supported | Supported | Supported | Supported | Supported | Unknown | Supported | Supported |
| Batch requests | Supported | Supported | Supported | Supported | Supported | Supported | Unknown | In testing | Supported |
| Notifications | Supported | Supported | Supported | Supported | Supported | Supported | Supported | Supported | Supported |
| Error responses | Supported | Supported | Supported | Supported | Supported | Supported | Supported | Supported | Supported |
| `error.data` | Supported | Supported | Supported | Supported | Unknown | Unknown | Unknown | Supported | Unknown |

## Safe Mode And Introspection

| Feature | JS Client | Node | Express | Python | PHP | .NET | Java | Arduino | Node-RED |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Safe Mode | Supported | Supported | Supported | Supported | Supported | Supported | Supported | In testing | Supported |
| Safe Mode strict mode | Supported | Supported | Supported | Supported | Supported | Unknown | Unknown | In testing | Unknown |
| Introspection | Not applicable | Supported | Supported | Supported | Supported | Supported | Supported | Supported | Supported |
| Schema metadata | Not applicable | Supported | Supported | Supported | Supported | Supported | Unknown | Supported | Supported |
| Runtime schema validation | Not applicable | Supported | Supported | Supported | Supported | Supported | Unknown | Planned | Supported |

## Transports And Integration

| Feature | JS Client | Node | Express | Python | PHP | .NET | Java | Arduino | Node-RED |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| HTTP transport | Supported | Supported | Supported | Supported | Implementation-specific | Supported | Supported | Supported | Supported |
| Serial transport | Not applicable | Not applicable | Not applicable | Not applicable | Not applicable | Not applicable | Not applicable | Supported | Not applicable |
| Authentication middleware/hooks | Not applicable | Implementation-specific | Supported | Implementation-specific | Supported | Supported | Supported | Not applicable | Supported |
| Method whitelist | Not applicable | Unknown | Supported | Unknown | Supported | Unknown | Unknown | Unknown | Unknown |
| Node-RED integration | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Supported |
| Embedded support | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Client-compatible | Supported | Client-compatible |

## Notes

- `rpc-arduino-toolkit` documents standard JSON-RPC support, Serial/UART, HTTP client/server transports, object and array params, custom errors with `error.data`, introspection, and Safe Mode HTTP interoperability in testing.
- `rpc-python-toolkit` is an early Python toolkit and documents Safe Mode, HTTP client/server handling, batch, notifications, introspection, and JSON Schema validation.
- `rpc-toolkit-js-client` is a client library, so server-side features such as introspection hosting and runtime validation are not applicable.
- `node-red-contrib-rpc-toolkit` integrates RPC endpoints into Node-RED flows and delegates core HTTP behavior through the Node-RED runtime and `rpc-express-toolkit`.
- Java and .NET documentation indicates broad feature support, but detailed interoperability status should still be verified by the shared conformance test matrix.
