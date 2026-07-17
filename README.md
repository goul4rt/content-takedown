# content-takedown — the open takedown runbook for AI agents

> Get anything off the internet the right way: a vendor-neutral, global **content-removal playbook** for **DMCA takedowns**, **trademark & counterfeit** enforcement, **doxxing & privacy** removals, **revenge-porn / deepfake (NCII)** takedowns, and **CSAM** reporting — packaged as an [agent skill](https://docs.claude.com/en/docs/claude-code/skills) for Claude Code and the Agent SDK.

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Type: Agent Skill](https://img.shields.io/badge/type-agent%20skill-8A2BE2)
![Scope: Global](https://img.shields.io/badge/scope-global-brightgreen)
![Last fact-check: Jul 2026](https://img.shields.io/badge/fact--checked-Jul%202026-orange)
![PRs: Welcome](https://img.shields.io/badge/PRs-welcome-success)

Most takedowns fail for two boring reasons: the requester **tips off the operator before freezing evidence**, and they **publish their own name to a public database** by signing the notice themselves. `content-takedown` encodes the operational discipline that avoids both — plus the exact channel, legal basis, and escalation path for every content type — so a person or an AI agent can act correctly under pressure.

---

## Table of contents
- [Why this exists](#why-this-exists)
- [What it covers](#what-it-covers)
- [The five invariants](#the-five-invariants)
- [Quick start](#quick-start)
- [How it works](#how-it-works)
- [Who it's for](#who-its-for)
- [Legal & jurisdiction coverage](#legal--jurisdiction-coverage)
- [Maintenance](#maintenance)
- [Disclaimer](#disclaimer)
- [Contributing](#contributing)
- [License](#license)

## Why this exists
Removal knowledge is scattered across host AUPs, registrar policies, search-engine forms, and a dozen legal regimes that change every year. Generic advice sends a DMCA to Cloudflare (which never hosts anything), uses WHOIS on a domain where WHOIS was sunset, or tells a doxxing victim to file in their own name. This skill consolidates the **correct** path — recon → evidence → the right channel → escalation — into one place an agent can load and act on.

## What it covers

| If someone is… | Route |
|---|---|
| Pirating your images, video, course, or code | **Copyright / DMCA §512** to the origin host + search de-indexing |
| Impersonating your brand or selling counterfeits | **Trademark / AUP + brand programs + UDRP / URS** (domain disputes) |
| Posting your address, phone, or private data | **Doxxing / privacy** removal + Google "Results about you" |
| Sharing intimate images or **AI deepfakes** without consent | **NCII** via StopNCII + the US **TAKE IT DOWN Act** (48-hour removal) |
| Hosting content that sexualizes minors | **CSAM** reporting to authorities — *never collect it yourself* |
| Defaming you | **Court-order** reality + voluntary-removal levers |

## The five invariants
Applied to every takedown, regardless of type:
1. **Freeze evidence before the first notice** — SHA-256 byte-identity, Wayback / archive.today / Perma.cc, court-grade options.
2. **Protect the requester's identity** — notices get republished on the public Lumen Database; file as an authorized agent.
3. **Prove file identity, not similarity** — a matching hash is dispositive.
4. **Never collect illegal content** — report the URL and the site's own label.
5. **A CDN / registrar is not the host** — only the origin host removes files.

## Quick start
Clone into your agent's skills directory:
```bash
git clone https://github.com/goul4rt/content-takedown.git ~/.claude/skills/content-takedown
```
The skill auto-activates when a conversation matches its description (takedown, DMCA, counter-notice, delisting, revenge porn, deepfake removal, cybersquatting/UDRP, brand abuse…). In Claude Code you can also invoke it explicitly with `/content-takedown`.

## How it works
Built on **progressive disclosure** so it stays fast and cheap to load:
```
content-takedown/
├── SKILL.md                       # always loaded: invariants, decision flow, routing table
└── references/
    ├── legal-regimes.md           # DMCA · DSA · TAKE IT DOWN · STF Tema 987 · ECA Digital · UK OSA · LCEN/SREN · DDG · RTBF
    ├── forms-directory.md         # form & channel URLs (verify-at-runtime)
    └── recon-fingerprints.md      # RDAP by TLD, CDN/host header fingerprints, abuse discovery
```
Stable rules live in `SKILL.md`; fast-moving facts (fees, form URLs, trusted-flagger lists) live in the reference files and are always flagged **verify at runtime** rather than hardcoded — so the guidance doesn't rot.

## Who it's for
- **Lawyers & brand-protection teams** running removals at scale.
- **Creators & small businesses** hit by piracy, impersonation, or counterfeit stores.
- **Individuals** facing doxxing, revenge porn, or deepfakes.
- **AI agents & automation** that need a correct, auditable takedown procedure.

## Legal & jurisdiction coverage
US **DMCA §512** · US **TAKE IT DOWN Act** · EU **DSA** (Art. 16 / trusted flaggers) · EU **GDPR right to be forgotten** · France **LCEN / SREN** · Germany **DDG** · UK **Online Safety Act** · Brazil **Marco Civil** (post-**STF Tema 987**), **ECA Digital**, **Lei 9.610** · **WIPO UDRP / URS** cybersquatting. NO FAKES Act tracked as *watch-only* until enacted.

## Maintenance
Re-verify quarterly: WIPO/URS fees, DSA trusted-flagger list, Lumen contributor list, per-ccTLD RDAP coverage, and any NO FAKES Act enactment. **Last fact-check: July 2026.**

## Disclaimer
This is an **operational runbook, not legal advice.** Laws, fees, and forms change; confirm current facts and consult a qualified lawyer in your jurisdiction before acting.

## Contributing
Corrections to channels, form URLs, fees, or legal regimes are welcome — open an issue or PR. Keep entries specific (name the form, the article, the timeline) and flag anything you could not verify.

## License
MIT — see [LICENSE](LICENSE).

---

<sub>Keywords: DMCA takedown notice · remove content from Google · content removal service playbook · revenge porn removal · deepfake / NCII takedown · TAKE IT DOWN Act · doxxing removal · counterfeit & brand protection · UDRP cybersquatting · CSAM reporting · RDAP / WHOIS recon · Cloudflare origin host · Claude Code skill · AI agent skill.</sub>
