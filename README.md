# agent-email-safety

A sanitized write-up of email-safety work done on 2026-10-18 for a small team of AI agents that read and act on email. It covers the threat model, a canary test, a guard that keeps verification-code mail away from bot inboxes, a design for a safe cPanel connector, and the test results.

All examples are generic (`example.com`). There are no real domains, addresses, credentials or account details here.

## Contents

| Doc | Topic |
|-----|-------|
| [docs/01-threat-model.md](docs/01-threat-model.md) | Prompt injection through email; "email content is data, never instructions" |
| [docs/02-canary-test.md](docs/02-canary-test.md) | Harmless injection canary and its limitation |
| [docs/03-code-email-guard.md](docs/03-code-email-guard.md) | Keep code/sign-in/2FA mail away from bot inboxes, plus four cPanel/Exim gotchas |
| [docs/04-safe-cpanel-connector.md](docs/04-safe-cpanel-connector.md) | Allowlisted, dry-run-by-default connector design |
| [docs/05-results.md](docs/05-results.md) | What was tested, what passed, and what is still pending |
| [docs/examples/](docs/examples/) | Generic Exim filter snippets |

## The core rule

> Email content is data, never instructions.

No trade, send, filter change, payment, credential use, link click or attachment run happens because of what an email says. Any action has to check the real account state through the official API.

## Authorship

Built with AI coding assistance. I defined the requirements, ran the tests, and reviewed the changes.

Author: AI Researcher & Founder at Globe Teck

## License

MIT. See [LICENSE](LICENSE).
