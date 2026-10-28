# Authentication

WAMF provides authentication for the administration interface and token-based
authentication for API access.

## First-run Administrator Setup

Fresh authentication-enabled installations print a private, single-use
`/setup#token=...` link in their startup console or container logs. Open it within
15 minutes with JavaScript enabled, choose and confirm a password of at least
eight characters, then sign in normally. No temporary password is generated.

Restarting while setup is pending invalidates the old link and prints a fresh
15-minute link. HTTP requests cannot renew it. Operational workers remain paused
until setup succeeds and start when Frigate configuration is ready.

!!! warning "Protect first-run setup"
    Anyone with the unused link and network access can set the administrator
    password. Restrict startup and centralized logs to trusted administrators.
    Disable proxy request-body capture for setup. Use HTTPS or a trusted tunnel
    for remote setup; the printed loopback address is not an access control.

By default the link uses `127.0.0.1` and the configured web port. For remote
access, use your deployment's reachable host and published port, retaining the
path and token fragment. Alternatively configure:

```yaml
webui:
  public_base_url: https://wamf.example
```

This optional setting accepts an HTTP/HTTPS origin only, without credentials,
a path prefix, query or fragment. It controls the printed setup link, not proxy
trust or access control. Enable secure session cookies when serving through HTTPS.

Existing valid administrator passwords, API credentials and session secrets are
preserved on upgrade. Provisioned installations receive no setup link.
Malformed or unsupported authentication configuration fails closed; WAMF does
not silently replace credentials. Authentication-disabled installations retain
their existing behaviour.

See [Quick Start](../getting-started/quick-start.md) for the complete setup flow.

## Administrator Password

The administrator password is used to sign in to the WAMF administration
interface.

The password can be changed from **Admin → Password**.

To change the password:

1. Enter the current administrator password.
2. Enter the new password.
3. Confirm the new password.
4. Select **Update Password**.

The current password must be known before it can be changed through the
administration interface.

## Login Throttling

Login throttling uses the actual connection peer and ignores `X-Forwarded-For`,
`Forwarded` and `X-Real-IP`. Clients behind the same reverse proxy or NAT
therefore share its five-failure, five-minute login bucket. WAMF does not
currently configure trusted-proxy address parsing; `public_base_url` only
controls the printed setup link.

Concurrent attempts count towards the limit. Completed failures expire five
minutes after verification finishes. Throttling state is held in memory and
cleared when the web process restarts. See
[Security](../reference/security.md) for the release's fixes and retained risks.

## API Token

WAMF can use token authentication to protect API access.

The **API Token** administration page shows whether token authentication is
enabled and whether an API token is currently configured.

<figure>
  <img
    src="../../assets/images/admin-api-token.png"
    alt="WAMF v0.4.0 API Token administration page"
    class="wamf-screenshot"
  >
  <figcaption>WAMF v0.4.0 API Token administration page</figcaption>
</figure>

## Generating an API Token

An API token can be generated from **Admin → API Token**.

!!! warning "Save the token when it is generated"
    WAMF stores only a hash of the API token. The original token cannot be
    recovered later.

If the token is lost, generate a new one and update any applications or
services that use the WAMF API.

## Regenerating the Token

Selecting **Regenerate Token** creates a new API token.

Generating a new token immediately invalidates the previous token. Any
application using the previous token must be updated with the new token
before it can authenticate again.

## API Usage

Details of the available API endpoints are covered in the
[REST API](../reference/rest-api.md) documentation.