# Findings

!!! warning "Fictional training data"
    These findings come from a deliberately vulnerable practice web app, run privately on one machine. It stands in for the Aura Workforce application. No real system was tested, and no exploit steps are published here.

## How the findings were produced

An automated passive scan (ZAP baseline) was run first. Every alert was then checked by hand, and the results were compared with earlier manual testing. A finding is marked **Real** only after it was reproduced.

## Verified findings, ranked by severity

| # | Finding | Verdict | Severity | Fix |
|---|---|---|---|---|
| 1 | Script runs from the search box (DOM-based XSS) | Real, confirmed by hand. The scanner rated it Low from a code pattern only. | **High** | Never pass user input into trusted-HTML functions. Sanitise or escape it. Add a Content Security Policy. |
| 2 | Internal files readable without login | Real, missed by the scanner | **High** | Remove the public route, keep files outside the web root, require login. |
| 3 | Content Security Policy header missing | Real | **Medium** | Add the header, starting in report-only mode. |
| 4 | Detailed error page leaks internal information | Real, missed by the scanner | **Low** | Show a generic error page. Keep traces in server logs only. |
| 5 | Cross-origin access allowed for every site | Real, low impact | **Low** | Restrict to the app's own origin. |
| 6 | Cross-Origin-Embedder-Policy header missing | Real, hardening only | **Low** | Add the header and test that embeds still work. |
| 7 | Cross-Origin-Opener-Policy header missing | Real, hardening only | **Low** | Add the header. |
| 8 | Deprecated browser-feature header in use | Real, trivial | **Info** | Replace it with the current Permissions-Policy header. |
| 9 | Cache alerts and "modern web application" note | Not vulnerabilities, or false alarms | **Info** | None. |

### Rating notes

- Finding 1 is High because it needs no login, works from one crafted link, and runs script in the victim's browser.
- Finding 2 is rated High, not Critical: the most sensitive file was an encrypted vault, but downloading it still allows unlimited offline guessing, and another file was readable in plain text. The rating would go up if the vault password were weak.

## Other issues found by manual testing

These were found by hand because a passive scan cannot see them. Details are kept private.

- One user could read another user's shopping basket by changing an ID (broken access control).
- Login could be bypassed with crafted input (SQL injection).
- A login token contained a password hash.
- Several old software dependencies were in use.
- The password reset question was weak, which allowed account takeover.
- There was no account lockout after repeated wrong passwords.
- A shopping basket accepted a negative quantity (business-logic flaw).
- Too little logging existed to trace these actions.

Each of these is tracked in the [risk register](risk-register.md) with an owner and a due date.

## Scanner versus manual testing

| Where the scanner helped | Where the scanner failed |
|---|---|
| Fast check of headers and configuration | Wrongly passed several checks that were real (open internal files, detailed errors, server-side old packages) |
| Found missing security headers and the cross-origin setting | Cannot log in, so it misses access control, injection, authentication and business-logic flaws |
| Cheap to repeat on every build | Also produced noise: junk URLs and cache alerts |

**Lesson:** a scan with no High results does not mean a system is safe. The most serious items came from manual testing. Use both, and check every alert before reporting it.