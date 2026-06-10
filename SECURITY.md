# Security Policy

This repository is a documentation hub for the RPC Toolkit ecosystem. Security handling, supported versions, and release policies are implementation-specific and may differ between repositories.

## Reporting Vulnerabilities

Please do not disclose security issues in public issues before they have been reviewed.

Use the maintainer contact published in the relevant implementation repository. If the issue affects this ecosystem hub or multiple repositories, contact Nicola Carpanese at `nicola@carpanese.com`.

If a dedicated security contact is published later, prefer that channel.

## Security Principles

- Safe Mode is not authentication, authorization, signing, TLS, or encryption.
- Introspection should be controlled in production.
- Hiding a method from introspection is not enough; authorization must be enforced at call time.
- Embedded devices should be protected by network boundaries, gateways, VPNs, reverse proxies, or equivalent controls.
- Detailed schemas and method metadata should be exposed only when intended.

## Scope

For implementation-specific vulnerabilities, report against the affected repository when possible:

- `rpc-express-toolkit`
- `rpc-node-toolkit`
- `rpc-python-toolkit`
- `rpc-php-toolkit`
- `rpc-dotnet-toolkit`
- `rpc-java-toolkit`
- `rpc-arduino-toolkit`
- `rpc-toolkit-js-client`
- `node-red-contrib-rpc-toolkit`
