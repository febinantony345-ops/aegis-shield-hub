# Detections

!!! warning "Fictional training data"
    The log data behind these rules is invented for a training project about a fictional company, Aura Workforce. Exact alert thresholds are deliberately not published.

Twelve detection rules were written and tested against three sets of logs: sign-in events, payroll audit events and web access logs. Every planted test event in the data was found.

## Rules

| Rule | What it detects | First checks for the responder |
|---|---|---|
| A1 | Brute force: repeated failed logins on one account | Did a success follow the failures (see A5)? Lock the account and reset the password. |
| A2 | Password spraying: one address failing on many accounts | Block the address. Check whether any of those accounts later had a success from it. |
| A3 | Impossible travel: logins from two countries too close together in time | Call the user. Was MFA used on the second login? Kill sessions and reset the password. |
| A4 | Successful login outside working hours (one alert per user per night) | Was MFA used? Is the address known for this user? Ask the user or manager and check what they did after logging in. |
| A5 | Success from an address that had many earlier failures | Treat the account as compromised: disable it and review its actions since the login. |
| P1 | Full payroll export | Who ran it, when, and was there a ticket? Check where the file went. |
| P2 | Salary change with no approval | Reverse the change, find the approver, and check with HR whether it was legitimate. |
| P3 | Burst of salary views by one person | Compare with this user's normal behaviour, ask the manager, and check which employees were viewed. |
| W1 | SQL injection, cross-site scripting or path traversal patterns in web requests | Check the status code (200 or 500 means it reached the app), review the app logs, and block the address. |
| W2 | Web login brute force | Block the address, check for a success after the rejections, and review the targeted account. |
| W3 | Scanning: many "not found" responses from one address | Block the address and check whether any probe returned a real page. |
| W4 | Probing for sensitive paths (admin pages, configuration files, source folders, backups) | Check whether any request succeeded and whether the file really exists. |

## How the rules were tuned

Thresholds were set from the busiest normal activity in the same logs, with headroom, so normal behaviour does not raise alerts. Exact values are kept private, because published numbers tell an attacker how much activity stays under the alert.

Rule A4 groups logins into one alert per user per night, so a single incident is one alert and not several.

## Handling a legitimate exception

Suppose a trusted administrator really does log in at an unusual hour for a scheduled job. The rule is not switched off. Instead:

- A narrow exception is added that matches only when the user, source, country and time window all fit. A login that differs on any of these still alerts.
- The account requires MFA.
- A dedicated account is used for that job and for nothing else, and an alert fires if it does anything other than the job.

## Where this connects

Each rule links to a playbook in the incident response policy and to a threat in the [policies](policies.md) page, so an alert always has a defined next step.