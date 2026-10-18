# 6. Finding: credential near-miss

## What happened
During setup, an API token was saved in the OS keychain item's **account (username) field** instead of its **secret field**.

## How it was caught
A pre-flight check runs before any live call. It flagged the item because the account value looked like a token (the expected length and character pattern) when it should have been an ordinary identifier.

## Exposure
One diagnostic printed that field once, inside the local session. It didn't end up anywhere public: no repo, logs or chat.

## Remediation
1. Revoked the token.
2. Created a new token.
3. Stored the new one in the secret field using the interactive secret prompt, so it never shows up in shell history.

## Lessons
1. **Check a credential's shape and placement before first use.** Confirm the username isn't token-shaped and the secret is present, without printing either. Report only length and whether it matches the pattern, for example: `account: len=12, token-shaped=no; secret: present=yes, len=40, token-shaped=yes`.
2. **Any exposure means rotation, even a local one.**
3. **Tooling should never echo credential fields.** Some keychain lookup commands print the account field even when you don't ask for the password, so a "safe" lookup can still leak a token that was stored in the wrong field.
4. **Login route:** use the provider's main portal and the standard HTTPS 443 proxy endpoint (for example `cpanel.example.com`), not non-standard admin ports, which restricted networks often block.
