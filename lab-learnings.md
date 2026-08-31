# Lab Learnings

These notes cover PwnBox labs I completed.

## Solved challenges

- Authentication proof that was not bound tightly enough to the account or transaction
- A digest comparison that did not provide the authenticity guarantee the application expected
- Browser messages accepted without the required origin and event checks
- A protected operation name exposed through schema suggestions
- SQL injection that reached the challenge data store
- A URL validator and browser interpreting the same destination differently
- A legacy numeric lookup that remained available after opaque identifiers were introduced
- Local-file fetching through XML external entity behaviour
- Object-level authorization failures
- Unsafe raw HTML and filename handling
- Active SVG upload handling

## How I worked through them

I kept notes on what I tried and what changed in each response. I tracked flag recovery separately from platform acceptance because they did not always happen at the same time.

## What I use now

- Parse a URL before deciding whether its destination is trusted.
- Bind authentication proof to the account and the exact transaction.
- Test old identifiers when an authorization system is migrated.
- Validate browser messages where the sensitive action is taken.
- Treat record ownership as an authorization decision.
- Keep an unresolved theory out of the verified-results list.
