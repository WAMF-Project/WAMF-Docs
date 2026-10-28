# Security in WAMF v0.4.2

v0.4.2 fixes specific security issues while retaining documented compatibility
and deployment risks. It is not a vulnerability-free release. This page
summarizes the release-specific records; it does not certify every deployed
image or native environment.

## Security Fixes

- **Detection rendering and event identifiers (F-001):** detection values are
  treated as DOM/text data rather than executable HTML or inline handlers.
  Frigate event identifiers are validated before ingestion, media requests,
  publication and archiving.
- **Login throttling (F-004):** spoofable forwarding headers no longer determine
  the client identity. Concurrent attempts and overflow state retain their
  limits. Clients behind one proxy or NAT share a bucket; see
  [Authentication](../administration/authentication.md#login-throttling).
- **Requests and diagnostics:** media-proxy paths, redirects, cleanup and
  forwarded headers are hardened; internal login destinations and bounded
  Wikipedia redirects are validated. Corrected responses/logs avoid YAML source
  excerpts, credential-bearing exceptions and unescaped control characters.

## Browser Assets

**F-005:** Monaco Editor 0.52.2 and its workers are served locally with the
original MIT licence, third-party notices and checksum/provenance manifest.

Bootstrap and jQuery remain external CDN assets. Their tags use SHA-384
Subresource Integrity and anonymous CORS. Integrity checks verify expected
content; they neither provide CDN availability nor establish that the selected
library versions are vulnerability-free. Do not assume the entire interface
works offline.

See the [asset integrity record](https://github.com/WAMF-Project/WAMF/blob/v0.4.2/docs/cdn-integrity.md).

## Retained Protobuf Risk

The release retains protobuf 3.20.3 and the documented deferral for
CVE-2025-4565 and CVE-2026-0994. TFLite Support 0.4.4 requires protobuf below 4,
so an isolated major protobuf upgrade is not included.

The trusted bundled-model boundary does not remove these vulnerabilities.
See the [compatibility decision and limitations](https://github.com/WAMF-Project/WAMF/blob/v0.4.2/docs/release-0.4.1.md#known-security-limitations).

## Local Packaging Tools (F-006)

The release records a scoped risk acceptance for local pip/setuptools exposure,
finalized on 27 October 2026. No execution path through normal WAMF operation
was identified, but affected tools remain relevant during package downloads,
installation and distribution-building operations.

Keep packaging tools reviewed and current, avoid untrusted indexes and
artifacts, and plan tooling maintenance before processing untrusted inputs.
The acceptance is not a claim that retained tools are safe for arbitrary
packages.

The Docker build checks dependencies and removes packaging tools and ensurepip
from the merged runtime filesystem. That mitigation depends on their actual
absence; it does not certify older or independently built images, erase
inherited layers, or constitute a new v0.4.2 image scan.

See the [F-006 assessment](https://github.com/WAMF-Project/WAMF/blob/v0.4.2/docs/security-0.4.2-f006.md).

## First-run Setup Links (F-007)

The setup link is a credential. Anyone who obtains the unused link and can reach
WAMF within its lifetime can set the administrator password. Expiry and single
use limit exposure but do not remove retained log copies.

The conditional acceptance finalized on 27 October 2026 requires operators to:

- Restrict startup logs, console recordings and exported copies to trusted
  administrators authorized to perform setup.
- Protect access to centralized log collectors, dashboards and exports.
- Disable setup request-body capture in proxies, tracing systems and WAFs: the
  POST body contains the token and chosen password.
- Use HTTPS or a trusted tunnel for remote setup.
- Complete setup promptly and keep the unused link private.

The printed loopback URL does not restrict access to localhost. Network
exposure depends on the listening address, published ports, proxies and
firewall rules. Where log access and the other conditions cannot be adequately
controlled, this risk acceptance does not apply.

See [Authentication](../administration/authentication.md) for setup instructions
and the [F-007 assessment](https://github.com/WAMF-Project/WAMF/blob/v0.4.2/docs/security-0.4.2-f007.md)
for the full conditions.

## Docker and Other Retained Limitations

The v0.4.1 Docker hardening is carried forward: a reviewed base digest,
build-time Debian updates, constrained dependencies, dependency checking and
removal of unused multimedia components and packaging tools.

The container still runs as root and retains the existing Flask runtime and
supported legacy credential compatibility. Debian/native-library limitations
also remain. Configuration saves require a writable directory mount and use
mode-0600 atomic replacement; see [Docker](../deployment/docker.md).

Historical image findings and measurements belong to their recorded builds.
Source-test success and a working deployment do not establish that the
published image has no vulnerabilities. The tagged v0.4.2 release record still
contains outstanding image-verification conditions; no completed new image
scan is asserted here.

## Release and Upgrade Records

- [v0.4.2 release summary](../releases/0.4.2.md)
- [Updating WAMF](../deployment/updating.md)
- [Tagged v0.4.2 security and release record](https://github.com/WAMF-Project/WAMF/blob/v0.4.2/docs/release-0.4.2.md)
- [Historical v0.4.1 image limitations](https://github.com/WAMF-Project/WAMF/blob/v0.4.2/docs/release-0.4.1.md#known-security-limitations)
