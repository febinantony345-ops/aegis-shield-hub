# Sensitive-content checklist (fictional training project)

Run before every publish. Every box must be ticked.

## Never publish

- [ ] Secrets: passwords, API keys, tokens, private keys, `.env` files, encryption keys
- [ ] Working exploit steps or attack strings that would work on real systems
- [ ] Real personal data: real names, email addresses, phone numbers, salaries, ID or bank numbers
- [ ] The vulnerable practice lab, its container files or its scan output
- [ ] Internal addresses, ports, server paths or exact software versions of any real system
- [ ] Exact alert thresholds and allowlist details for detection rules
- [ ] Screenshots that show a browser profile, local file paths, tabs or notifications

## Must be present

- [ ] A visible "fictional training data" note on the home page and on every page that shows data
- [ ] Author shown as anonymous (no real name on any page)
- [ ] Every finding has a fix, not only a problem
- [ ] No page links to a private repository

## Checks run

- [ ] Leak scan (search for secrets, addresses, names) shows no unexplained hits
- [ ] Link check passes
- [ ] Spell check passes
- [ ] Every page opened on a phone

## The three I would never publish, even in a fictional project

1. **Secrets (keys, passwords, tokens).** Once public they can be copied in seconds by bots, and deleting them later does not help because Git history and caches keep them. Real habits form from fake projects, so the habit matters even when the secret is fake.
2. **Working exploit steps for a real system.** A finding should say what is wrong and how to fix it. A copy-paste attack recipe helps attackers more than defenders.
3. **Real personal data.** A person cannot take it back once it is public, and a "fictional" dataset is often built from real records without anyone noticing.

The vulnerable lab is also never public. It is a running, deliberately broken system. Publishing it would be a breach of the rules.