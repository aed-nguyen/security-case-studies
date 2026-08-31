# Automation Security Review

This review covered an internal automation system that accepted inbound work, created records in other services, retried failed jobs, and gave staff a way to repair incomplete work.

## Context

I reviewed the points where a valid request could still affect the wrong record, two jobs could collide, or a partial failure could leave staff without enough information to recover safely.

## Scope

The review covered 138 tracked files. The starting revision passed 1,125 tests across 82 test files, a closed-configuration check, and a deployment dry run. Its production dependency audit had no known advisories.

## Findings

| Problem | Consequence | Fix |
| --- | --- | --- |
| A legacy repair path could run without strong enough evidence connecting the message to the record | A valid repair request could affect the wrong record | Fail closed and bind the claim to the owner, record, action, source event, and expiry |
| Message filters decided whether inbound work was eligible | The filters didn't prove where the request came from | Require origin evidence before admitting the work |
| Concurrent jobs could create the same external record | Two valid jobs could leave duplicate or conflicting records | Use owner-bound leases and checked state changes around the create |
| The queue had no tight per-account or system-wide bound | A valid source could admit more work than the system could safely process | Cap admitted work and keep terminal states explicit |
| Partial external changes didn't leave enough recovery information | A later repair could repeat the wrong step or lose track of what changed | Save the minimum data needed for a safe compensating action |
| Native error text could reach logs directly | Private or internal details could escape through unexpected exceptions | Log an allowed error class and an incident reference |
| Tracked test fixtures contained identifying values | Test data could expose information and keep returning in later changes | Replace the fixtures with invented data and scan tracked files for reintroduction |

## Results

The follow-up implementation added focused regression tests and a security-hardening migration. The passing test count increased from 1,125 to 1,138.
