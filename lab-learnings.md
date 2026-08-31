# Lab Learnings

## Hacker101 CTF

My [Hacker101 CTF](https://ctf.hacker101.com/) dashboard shows 31 points and 11 flags.

| Level | Flags |
| --- | ---: |
| A Little Something to Get You Started | 1 / 1 |
| Micro-CMS v1 | 4 / 4 |
| Micro-CMS v2 | 2 / 3 |
| Postbook | 4 / 7 |

I worked with page, post, profile, and account identifiers instead of assuming the application would enforce ownership everywhere. I tested edit and view routes separately, checked pages that weren't linked in the normal interface, and compared what changed before and after signing in.

The 31-point result also put my account above Hacker101's 26-point mark for private-program invitations.

## PwnBox

At my last checkpoint I had recovered 19 Easy flags. The solved work included:

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

I kept notes on what I tried and what changed in each response. I tracked flag recovery separately from platform acceptance because they didn't always happen at the same time.

That work changed how I test. I parse a URL before deciding whether to trust its destination. I bind authentication proof to the account and the exact transaction. I test old identifiers after an authorization system is migrated, and I check ownership at the point where a record is read or changed.
