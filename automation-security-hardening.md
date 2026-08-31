# Automation Security Review

I reviewed an internal automation system. I left out the organization and customer information. Repository paths, internal addresses, infrastructure identifiers, and secrets aren't included either.

## Why I reviewed it

The automation accepted inbound work and created records in other services. It also retried failed jobs and gave staff a way to repair incomplete work.

I needed to know more than whether an outside request could reach an endpoint. A valid request could still affect the wrong record. Two valid jobs could collide. A partial failure could leave the next person without enough safe information to recover.

## What I checked

The review covered 138 tracked files. The starting revision passed 1,125 tests across 82 test files, a closed-configuration check, and a deployment dry run. Its production dependency audit had no known advisories.

## Problems I verified

| Problem | Why it mattered | Fix |
| --- | --- | --- |
| A legacy repair path could run without strong enough evidence connecting the message to the record | A valid repair request could affect the wrong record | Fail closed and bind the claim to the owner, record, action, source event, and expiry |
| Message filters decided whether inbound work was eligible | The filters didn't prove where the request came from | Require origin evidence before admitting the work |
| Concurrent jobs could create the same external record | Two valid jobs could leave duplicate or conflicting records | Use owner-bound leases and checked state changes around the create |
| The queue had no tight per-account or system-wide bound | A valid source could admit more work than the system could safely process | Cap admitted work and keep terminal states explicit |
| Partial external changes didn't leave enough recovery information | A later repair could repeat the wrong step or lose track of what changed | Save the minimum data needed for a safe compensating action |
| Native error text could reach logs directly | Private or internal details could escape through unexpected exceptions | Log an allowed error class and an incident reference |
| Tracked test fixtures contained identifying values | Test data could expose information and keep returning in later changes | Replace the fixtures with invented data and scan tracked files for reintroduction |

## What I verified afterward

The follow-up implementation fixed all seven findings, added focused regression tests and a security-hardening migration, and increased the passing test count from 1,125 to 1,138.

That number records the test run I checked. It doesn't describe the system today.
