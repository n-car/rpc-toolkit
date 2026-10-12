# Releases

[Documentation home](index.md) · [Getting started](GETTING_STARTED.md) · [Validation](VALIDATION.md)

## Registry Snapshot

**Last checked: 2026-10-12.** This is a dated snapshot, not live registry status.
Availability was checked separately from successful uploads and GitHub tags.
Use the registry links in [Getting started](GETTING_STARTED.md#install-for-your-runtime)
to check later releases.

| Implementation | Version | Registry status at last check |
| --- | --- | --- |
| Express (including the compatibility safe package) | `4.5.2` | Available on npm; artifact integrity verified |
| Node core | `0.2.2` | Available on npm; artifact integrity verified |
| .NET / ASP.NET Core | `1.2.1` | Both packages available on NuGet |
| Java modules | `0.3.0` | All four modules available on Maven Central |
| Python | `0.2.1` | Available on PyPI |
| PHP | `2.0.1` | Available on Packagist |
| Arduino / PlatformIO | `1.1.1` | Available on PlatformIO; Arduino Library Manager indexing not yet confirmed |
| Node-RED | `2.3.4` | Available on npm; artifact integrity verified |

The shared JavaScript client remains at `1.1.3`; it was not changed in this release
batch. The Arduino Library Manager status above only describes the last check;
it does not establish whether indexing has completed since then.

## Change Summary

- Align invalid envelopes, explicit null IDs, notifications, and Safe Mode payload
  handling across runtimes.
- Sanitize unexpected errors in the affected runtimes while retaining PHP's
  trusted-context protection.
- Fix Arduino HTTP 204 batch completion and ESP32 response-buffer stack use.
- Add optional per-client socket/DNS configuration and Android `useNetwork(network)`
  in Java/Android `0.3.0`, without changing the wire protocol or default network
  behavior.
- Raise Node-RED `2.3.4`'s Express toolkit requirement to `4.5.2` and document the
  public application-error contract for flow responses.

See each implementation's release notes for API-specific details. The
[validation snapshot](VALIDATION.md) records the tested combinations and hardware
scope; the [compatibility matrix](COMPATIBILITY.md) distinguishes tested behavior
from implementation-specific support.
