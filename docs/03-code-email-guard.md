# 3. Code-email guard

## Rule
Never forward verification-code, sign-in, one-time, 2FA, password or security-code mail to a bot inbox. A bot that receives those codes could be steered into using them, and an injection plus a live code is an account takeover.

The guard was put in place across three kinds of mail system:

- Gmail filters
- Outlook rules
- cPanel Exim filters

In each one, the guard matches typical subjects and bodies (`verification code`, `sign-in`, `one-time`, `2FA`, `password`, `security code`) and keeps them in the human inbox, with no forwarding.

## cPanel / Exim specifics
The guard filter goes **first** and ends with `finish`, so nothing after it can forward the message.

### Gotcha 1: address-level forwarders skip filters
A cPanel *forwarder* on an address runs outside the user filter chain, so the guard never sees the message. **Fix:** remove the forwarder and replace it with a filter-based redirect placed **after** the guard.

### Gotcha 2: an earlier `finish` stops the redirect
If an allowlist filter earlier in the chain ends with `finish`, the redirect is never reached for allowlisted mail. **Fix:** add the deliver/redirect action to that allowlist filter too.

### Gotcha 3: `deliver` cancels implicit local delivery
In Exim filters, a significant `deliver` means the message is no longer saved locally by default. **Fix:** add an explicit `save` to the inbox alongside the redirect.

### Gotcha 4: the save destination is `INBOX`
Use `INBOX`, not a full path and not `.INBOX`. cPanel prepends the mailbox path itself, and `.INBOX` creates a hidden subfolder where mail quietly piles up.

See [examples/](examples/) for generic snippets.
