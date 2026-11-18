# Executive summary

!!! warning "Fictional training data"
    Aura Workforce is a fictional company. Everything below comes from a training project, and no real organisation or person is described.

## What was done

A security review was carried out for Aura Workforce, a payroll and HR platform that holds pay, ratings and personal details. The work covered:

- **Risk assessment:** 22 risks scored and ranked, each with an owner and a due date.
- **Testing:** a private practice lab and a passive scan, with every alert checked by hand.
- **Policies:** 10 policies and 90 rules, each with an auditor check and a linked threat.
- **Detection:** 12 rules that find attacks in sign-in, payroll and web logs.

## Where things stand

The register holds 8 Critical, 11 High and 3 Medium risks. None is closed yet: fixes count only once they are built and tested on the real application, not only on the practice lab.

## Most urgent

Five risks tie at the top score, so they were ranked by evidence of real misuse, then by speed of fix:

1. **Leavers and shared accounts still active.** Disable them and close accounts automatically on the exit date.
2. **No multi-factor authentication** on admin, HR, payroll and finance accounts.
3. **Brute-force password guessing.** Add lockout, a longer minimum length and a breached-password check.
4. **SQL injection.** Use safe queries everywhere.
5. **Employees reading each other's salaries** by changing an ID. The server must check ownership on every request.

## Main lesson

An automated scan with no High results does not mean a system is safe. The most serious problems came from manual testing, so both are needed, and every alert must be checked before it is reported.

## Where to look

- [Findings](findings.md): what was wrong, how serious, and the fix.
- [Risk register](risk-register.md): the top five and how ties were ranked.
- [Policies](policies.md): the policy index, one full example and the threat-to-rule map.
- [Detections](detections.md): what each rule detects and what the responder checks first.
- [About this Hub](about.md): what is and is not published.