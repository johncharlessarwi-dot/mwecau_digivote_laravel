# Ballot integrity review points

This document defines review questions for changes that affect voter eligibility, ballot submission, tallying, or results. It is not a claim that any specific cryptographic design is implemented.

## Before an election

- Confirm the eligibility source and snapshot time are documented.
- Confirm each eligible voter can submit only the ballot permitted by the published election rules.
- Verify that ballot submission and administrative configuration changes have separate authorization paths.
- Review duplicate-submission and retry behavior.

## During an election

- Monitor rejected submissions and unusual activity without exposing ballot choices to unauthorized staff.
- Preserve append-only evidence for sensitive events where supported.
- Restrict access to raw voter data and exports.

## Tally review

- Reconcile eligible voters, submitted ballots, invalid ballots, and counted ballots.
- Independently verify the tally output before publication.
- Document any exception and its resolution without altering source evidence.