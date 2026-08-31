# Automation Security Review

I reviewed an internal automation system. I left out the organization and customer information. Repository paths, internal addresses, infrastructure identifiers, and secrets are omitted too.

## Why I reviewed it

The automation accepted inbound work and created records in other services. It also retried failed jobs and gave staff a way to repair incomplete work.

I needed to know more than whether an outside request could reach an endpoint. A valid request could still affect the wrong record. Two valid jobs could collide. A partial failure could leave the next person without enough safe information to recover.

## What I checked

- Authentication and session boundaries
- Authorization and record separation
- Forged or replayed requests
- Input validation and rate limits
- Audit records and error exposure
- Concurrent work
- Recovery after a partial external change

## Problems I verified

- A legacy repair path could continue without strong enough evidence connecting the message to the record it was changing.
- Inbound eligibility depended on message filters that did not provide complete proof of origin.
- Concurrent runs could race while creating the same external record.
- Work admission was not bounded tightly enough per account or across the system.
- A partial external change did not leave enough safe information for recovery.
- Native error text could reach logs too directly.
- Some tracked test fixtures contained identifiers that should have been synthetic.

## How I narrowed the fixes

- Fail closed when required authorization evidence is missing.
- Bind a claim to the record and permitted action. Include the owner and source event. Give the claim an expiry.
- Use owner-bound leases with checked state changes for concurrent work.
- Serialize external creates or make them safe to repeat.
- Bound the amount of admitted work and keep terminal states explicit.
- Save only the recovery data needed for a compensating action.
- Replace raw errors with an allowed class and an incident reference.
- Replace identifying fixtures with invented data. Scan tracked files so they do not return later.

## What I verified afterward

The reviewed revision passed its existing validation suite and deployment dry run. The follow-up implementation added focused regression tests and a security-hardening migration. The last implementation run I observed reported 1,138 passing tests.

That number records the state I checked at the time. It is not a claim that an old test run proves the current production system is unchanged or fully secure.
