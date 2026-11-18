# Policies

!!! warning "Fictional training data"
    These policies were written for a fictional company, Aura Workforce. Roles, dates and figures are invented for a training project.

The policy pack has **10 policies and 90 rules**. Every rule has an auditor check (how someone can prove it is followed) and a linked threat (why the rule exists).

## Policy index

| No. | Policy | Rules |
|---|---|---|
| 01 | Access Control and Least Privilege | AC-01 to AC-09 |
| 02 | Passwords and MFA | PW-01 to PW-09 |
| 03 | Data Classification and Handling | DC-01 to DC-09 |
| 04 | Encryption | EN-01 to EN-09 |
| 05 | Logging and Monitoring | LM-01 to LM-09 |
| 06 | Incident Response | IR-01 to IR-09 |
| 07 | Backup and Recovery | BR-01 to BR-09 |
| 08 | Secure Development | SD-01 to SD-09 |
| 09 | Third Parties and Cloud | TP-01 to TP-09 |
| 10 | Acceptable Use and Awareness | AU-01 to AU-09 |

## Example: Policy 01, Access Control and Least Privilege

| | |
|---|---|
| Owner | Security Lead |
| Version | 1.0 |
| Effective | 21 October 2026 |
| Review date | 21 October 2027 |

### Purpose

People and systems get only the access they need for their job, and nothing more. This protects Restricted and Confidential data such as pay, ratings and personal details.

### Scope

All Aura Workforce staff, contractors, user accounts, service accounts, the web app, the API, the payroll engine, the ledger, the dashboards and the database.

### Rules

| ID | Rule | How an auditor checks it | Threat |
|---|---|---|---|
| AC-01 | Every account belongs to one named person or one named service. Shared accounts such as "admin" or "payroll" are not allowed. | Export the user list: every username maps to an employee ID or a service owner. | T-16 |
| AC-02 | Access is given by role. The roles and what each may see are written in an access matrix approved by the data owner. | The access matrix exists with approver and date. Check 5 users: their rights match the matrix. | T-06 |
| AC-03 | The server checks the user's role and ownership on every request. Hiding a button or menu is not enough. | Log in as an employee and request another employee's payslip in 10 tests. All 10 return "access denied". | T-06, T-07, T-28 |
| AC-04 | Only the Payroll role can start a payroll run, and a second person approves it. | The logs of the last 3 payroll runs show two different user IDs for each run. | T-12 |
| AC-05 | Restricted columns (pay, ratings, date of birth, leave reason) can be read only by the roles named in the data classification worksheet. | Compare database permissions with the "who should access it" column of the worksheet. No extra roles have access. | T-06, T-07 |
| AC-06 | The API, payroll engine and ledger each use their own database account with only the permissions they need. No application uses an admin database account. | List the database accounts: there is one per service and none has admin rights. | T-10, T-20 |
| AC-07 | A dashboard shows a group only if it has at least 5 people. | Open a dashboard for a team of 2 people: it shows "not enough data". | T-27 |
| AC-08 | The account of a person who leaves is disabled within 1 working day of the exit date. | Compare HR exit dates with account disable dates for the last 10 leavers: all within 1 working day. | T-01 |
| AC-09 | Data owners review who has access every 90 days and sign the result. | Signed review records exist for the last 2 quarters. | T-07 |

### Roles

- **Data owner** (HR head for HR data, Finance head for ledger data): approves the access matrix and does the 90-day review.
- **Security Lead:** keeps this policy, approves exceptions, checks the evidence.
- **System administrator:** creates and removes accounts as the access matrix says.
- **Line manager:** requests access for their team and tells HR when someone leaves.

### Exceptions

Only the Security Lead may approve an exception, in writing. An exception lasts at most 30 days, is recorded in the exception register, and ends automatically on its date. Emergency ("break glass") access is logged and reviewed within 1 working day.

### Review

This policy is reviewed once a year, or after a security incident that involves access.

## Why each rule exists: top 10 threats mapped to rules

| Rank | Threat | Rules that reduce it | How the rules work together |
|---|---|---|---|
| 1 | T-06 Employee reads a colleague's payslip by changing the employee ID | AC-02, AC-03, AC-05, SD-03, SD-08, LM-02, LM-08, AU-04 | The server checks ownership (AC-03, SD-03) and tests prove it on every build (SD-08). Logs and alerts (LM-02, LM-08) catch any miss, and AU-04 makes snooping misconduct. |
| 2 | T-09 SQL injection on API to Database | SD-02, SD-07, SD-08, EN-06, PW-04, IR-04 | Safe queries stop it (SD-02). Scans catch gaps (SD-07, SD-08). If it still happens, encrypted Restricted columns and hashed passwords limit the damage (EN-06, PW-04), and containment starts within 1 hour (IR-04). |
| 3 | T-01 Stolen or guessed password used to log in | PW-01, PW-02, PW-03, PW-05, LM-01, LM-07, IR-05, AU-05, AU-06 | MFA, long passwords and lockout make guessing fail. Alerts show attacks (LM-07). A compromised account is disabled within 1 hour (IR-05). Training and phishing tests reduce theft (AU-05, AU-06). |
| 4 | T-27 Dashboard for a very small team reveals one person's pay or rating | AC-07, DC-02, AU-04 | A dashboard hides groups under 5 people (AC-07). The data owner reviews classification every 6 months (DC-02). Misuse is misconduct and alerts are investigated (AU-04). |
| 5 | T-10 One powerful database account used by the API | AC-06, DC-01, TP-04, TP-05, AU-09 | Each service has its own limited account (AC-06). Columns have levels (DC-01). Cloud databases are never public (TP-04) and cloud admins use MFA (TP-05). Administrators are trained (AU-09). |
| 6 | T-07 Employee sends a request that changes a salary figure | AC-03, AC-05, AC-09, SD-01, SD-03, SD-04, LM-03 | Server-side role checks and input checks block it (AC-03, SD-03, SD-04). A second person reviews code (SD-01). Every change to salary data is logged with old and new value (LM-03). Access is reviewed every 90 days (AC-09). |
| 7 | T-12 Non-payroll user starts a payroll run | AC-04, SD-03, LM-05 | Only the Payroll role can start a run and a second person approves it (AC-04). Endpoint tests prove the check (SD-03). Each run records who started and approved it (LM-05). |
| 8 | T-14 Payroll rows edited after the run | LM-03, SD-01, BR-02, BR-08, IR-04, IR-06 | Changes are logged (LM-03) and code changes are reviewed (SD-01). Point-in-time backups let the team compare and undo (BR-02, BR-08). Evidence is kept and containment is fast (IR-06, IR-04). |
| 9 | T-05 Stolen or forged session token used on the API | PW-07, EN-09, IR-05, AU-05, AU-06 | Short sessions and logout cancel tokens (PW-07). Tokens are signed, so a changed one is rejected (EN-09). A compromised account is cut off within 1 hour (IR-05). Training reduces theft (AU-05, AU-06). |
| 10 | T-32 Ransomware locks the database | BR-01, BR-03, BR-06, BR-07, BR-09, IR-01, IR-08, LM-09, SD-07, TP-08 | Daily backups with an offline copy (BR-01, BR-03) are tested every quarter (BR-06). Recovery targets and an offline plan make recovery fast (BR-07, BR-09). A response plan and a payroll fallback keep staff paid (IR-01, IR-08). Alerts, patching and supplier checks reduce the chance (LM-09, SD-07, TP-08). |