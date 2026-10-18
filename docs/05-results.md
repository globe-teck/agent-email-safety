# 5. Results

## Negative tests: passed on 5 sources
For each of five mail sources, a test code-style message ("Your verification code is …") was sent, and it was confirmed **not** to reach the bot inbox.

**Caveat:** some tests were self-sends (sender and recipient on the same account or system). Some providers handle self-sent mail differently, so this is weaker evidence than mail from an outside sender.

## Positive control: pending
A pass on the negative test doesn't prove the normal forward still fires. If forwarding were broken, the code mail would also "not arrive". The pending positive control sends an ordinary, non-code message along the same path and confirms it **does** reach the bot inbox.

## Open items
- Positive control on every source.
- A spoofing canary from an unauthenticated source (see [02-canary-test.md](02-canary-test.md)).
