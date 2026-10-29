# Election administrator access controls

Election administration privileges should be limited, time-bound where practical, and auditable.

## Access review

- Assign each administrator an individual account; prohibit shared credentials.
- Separate voter registration, election configuration, result review, and system administration duties where staffing allows.
- Require strong authentication for privileged accounts.
- Review active administrators before each election and remove access after their duty ends.
- Keep emergency access documented, approved, and reviewed after use.

## Sensitive actions

Require a reason and audit record for changes to voter eligibility, candidate records, election dates, ballot configuration, and result publication. Restrict exports containing voter information.

## Incident response

If an administrator account is suspected compromised, revoke active sessions, preserve logs, notify the election owner, and assess whether election integrity could be affected. Do not silently edit or delete audit evidence.