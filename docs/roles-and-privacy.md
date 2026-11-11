# Roles and privacy

[← Back to overview](../README.md)

e-QUAART is designed around the Philippine Data Privacy Act of 2012 (RA 10173). Access rules are enforced on the server for every request, not only hidden in the interface.

## Who sees what

| Role | Scope | Learner-level data | Can change |
|---|---|---|---|
| Teacher (class adviser) | own classes | yes | learners, classes, results for own classes, interventions |
| Master Teacher, Assessment Coordinator | own school | yes | as teacher, school-wide, plus verify, return and reopen |
| School Head | own school | yes | classes and verification; read-only on scores |
| District Supervisor | own district | aggregates only | nothing |
| Education Program Supervisor | division, assigned learning areas | aggregates only | nothing |
| Chief, Curriculum and Instruction | division | aggregates only | nothing |
| Division Administrator | division | aggregates only | reference data, standards, users, settings |
| ICT / System Administrator | none | no | accounts and settings only |
| Data Protection Officer | none | no | audit trail, retention, breach register |

## Controls

- **Segregation of duties.** A result must be verified by someone other than the person who encoded it.
- **Small-cell suppression.** Aggregate rows covering fewer than five learners are masked for aggregate-only roles, and intervention learners appear to them as "Learner n".
- **Transfers.** A learner can only be enrolled into a new class with both their LRN and last name, so an LRN alone can't be used to look anyone up.
- **Audit trail.** Sign-ins, every create, update and delete, workflow steps, imports, exports (flagged when they contain personal data) and every learner-record view. Result changes keep the old and new values.
- **Accounts and sessions.** Strong password hashing and policy, forced change on first sign-in, lockout after repeated failures, short-lived sessions with idle and absolute time-outs.
- **Privacy notice and acknowledgement** on first use; exports with learner data are marked confidential.
- **Retention policies and a breach register** with the 72-hour notification deadline, managed by the Data Protection Officer.
