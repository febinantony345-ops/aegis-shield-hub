# Risk register

!!! warning "Fictional training data"
    All figures, accounts and incidents on this page are invented for a training project about a fictional company, Aura Workforce.

The register holds **22 risks**: 8 Critical, 11 High and 3 Medium. Each risk is scored likelihood x impact (1 to 5 each, maximum 25). **Five risks tie at a score of 20**, so the order below uses a tie-break rule.

## Top five risks

| Order | Risk (score) | Why it matters | Fix | Owner | Due |
|---|---|---|---|---|---|
| 1st | R-19 Leavers and shared accounts (20) | 6 leavers still active (one logged in 157 days after exit), 2 shared accounts, 5 dormant accounts | Disable them now. Close accounts automatically on the HR exit date (AC-08). | IT manager | 20 Nov 2026 |
| 2nd | R-17 No MFA (20) | 5 admins and 18 Finance or HR accounts have no MFA. Detection rule A4 saw a login without it. | Turn on MFA for every Admin, HR, Payroll and Finance account (PW-01). | Security Lead | 27 Nov 2026 |
| 3rd | R-04 Brute force (20) | 420 failed logins on one account, then a success | Lockout after 5 wrong tries (PW-05), 12-character minimum, breached-password check | Development lead | 27 Nov 2026 |
| 4th | R-02 SQL injection (20) | Gives admin access with no password. Attempts reached the app in the log review. | Parameterised queries everywhere, scan with 0 injection findings, encrypt Restricted columns | Development lead | 27 Nov 2026 |
| 5th | R-01 IDOR (20) | Any employee can read a colleague's salary by changing an ID. Proven in the practice lab. | Server checks ownership on every request. 10 "access denied" tests pass on every build. | Development lead | 27 Nov 2026 |

## How five risks with the same score were ranked

1. **Proof first:** a risk with real evidence of misuse (an actual login or a successful attack) comes first. A risk with only failed attempts or lab evidence comes later.
2. **Quick fix first:** if the proof is equal, the risk that can be fixed in hours goes before the one that takes weeks.
3. **Shared fix first:** if the fix times are equal, the fix that also lowers other risks goes first.
4. **Impact last:** if everything above is equal, the higher impact goes first.

## What would change each rating

| Risk | What would LOWER it | What would RAISE it |
|---|---|---|
| R-19 | The 13 old accounts are disabled and accounts close automatically on the exit date. Score 4 to 8. | Logs show a leaver or shared account touching Restricted data. Score 25. |
| R-17 | A new review shows MFA on 100% of Admin, HR, Payroll and Finance accounts. Score 5 to 10. | Rule A4 shows an off-hours login with no MFA on a payroll account from an unknown address. Score 25. |
| R-04 | Lockout is built and tested, and MFA is on targeted accounts. Score 8. | The attacked account is an admin or payroll account with no MFA. Score 25. |
| R-02 | Safe queries everywhere, 0 scan findings, Restricted columns encrypted. Score 8. | Rule W1 shows an injection attempt that reached the app with status 200 or 500. Score 25. |
| R-01 | Ownership check on every request, 10 "access denied" tests pass on every build. Score 10. | A log shows a real request using another employee's ID and getting a 200. Score 25. |

## Notes

- Fixes count only once they are built and tested on Aura Workforce itself, not only on the practice lab apps.
- R-04 and R-17 come from the same event (one admin account with no MFA, hit by 420 failed logins). One fix, MFA, lowers both.
- R-01 was the number-one threat in the threat model. It is ranked fifth only because the logs show no attack on Aura yet. It did not become less serious.
- This register is a learning exercise. It is not legal advice.