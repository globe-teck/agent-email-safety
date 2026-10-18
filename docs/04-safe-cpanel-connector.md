# 4. Safe cPanel connector design

The goal is a tool that lets an agent manage mail filters without being able to do serious damage.

## Design
- **Allowlisted UAPI functions only:** list filters, add filter, update filter, list email accounts, list forwarders. Every other function is refused.
- **`dry_run` defaults to `true`.** A dry run returns the request it would make, with the token redacted. A real call needs an explicit `dry_run=false`.
- **No delete or password tools.** Deleting filters/forwarders and changing passwords aren't exposed.
- **Domain allowlist.** Calls for any domain not on the list are rejected.
- **Transport:** HTTPS on 443 through the `cpanel.<domain>` proxy when the usual cPanel ports are blocked.
- **Secrets:** the API token is read at runtime from the OS keychain. It never appears in argv, environment dumps, or logs.
- **Tests:** a suite checks that non-allowlisted functions and domains are rejected, that dry run is the default, and that the token is redacted in every output path.

## One-off destructive changes
Things like removing a forwarder are done as **single, reviewed calls outside the tool**, not by adding a delete capability.

## Lesson learned
A token was once stored in the wrong keychain field. See [06-credential-near-miss.md](06-credential-near-miss.md) for what happened and what we learned.
