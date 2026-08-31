# Lab Learnings

## Hacker101 CTF

### Progress

| Level | Flags |
| --- | ---: |
| A Little Something to Get You Started | 1 / 1 |
| Micro-CMS v1 | 4 / 4 |
| Micro-CMS v2 | 2 / 3 |
| Postbook | 4 / 7 |

### Strategies

I worked with page, post, profile, and account identifiers instead of assuming the application would enforce ownership everywhere. Edit and view routes needed separate tests, including pages that weren't linked in the normal interface and changes in behaviour before and after signing in.

## PwnBox

### Progress

19 Easy flags recovered.

### Topics

- Authentication proof that wasn't bound tightly enough to the account or transaction
- A digest comparison that didn't provide the authenticity guarantee the application expected
- Browser messages accepted without the required origin and event checks
- A protected operation name exposed through schema suggestions
- SQL injection that reached the challenge data store
- A URL validator and browser interpreting the same destination differently
- A legacy numeric lookup that remained available after opaque identifiers were introduced
- Local-file fetching through XML external entity behaviour
- Object-level authorization failures
- Unsafe raw HTML and filename handling
- Active SVG upload handling

### Lab Learnings

- Parse a URL before trusting its destination.
- Bind authentication proof to the account, action, and exact transaction.
- Test old identifiers after an authorization system is migrated.
- Check ownership where a record is read or changed.
- Treat browser messages as untrusted until their origin and event shape are checked.

## OverTheWire

[Read the OverTheWire notes](overthewire.md)
