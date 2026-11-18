# Hosting options

!!! warning "Fictional training data"
    This page compares ways to host a static site. The Hub itself holds only invented data.

Free-tier terms were checked on 18 November 2026. They change often, so check the provider's pricing page before relying on any figure.

## Comparison for a static site

| Option | Price | Main limits | Ease |
|---|---|---|---|
| GitHub Pages | Free for public repositories. Private repositories need a paid plan. | Site up to 1 GB, soft 100 GB bandwidth a month, soft 10 builds an hour | Very easy. Built into the repository. |
| Cloudflare Pages | Free | Unmetered bandwidth for static files, 500 builds a month | Easy. Connect the repository. |
| Netlify | Free tier, then paid | Credit-based: 300 credits a month, and sites pause when credits run out | Easy. Connect the repository. |
| Vercel | Free Hobby plan, then paid | 100 GB bandwidth a month. Hobby is for personal, non-commercial use only. | Easy. Connect the repository. |
| Small paid server | A small monthly fee | Fully under your control, but you patch and secure it yourself | Hard. Needs server skills and ongoing care. |

## Choice for this Hub

A documentation site is a few hundred kilobytes of static files with no secrets, so free static hosting is enough. A paid server would add a machine to defend for no gain.

## Monitoring evidence

The detection rules were run as scripts on sample logs. No live monitoring stack runs behind this Hub, because the laptop used for the project is too small for one. Evidence is shown as rule descriptions and test results, and the scripts are kept in a private repository. A real deployment would run monitoring on a small paid server.

## The vulnerable lab stays private

The practice lab runs only on a private machine, bound to the local address. It is never hosted, and the Hub repository does not contain it.