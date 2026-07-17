# content-takedown

> A vendor-neutral, global runbook for getting online content removed or de-indexed: DMCA copyright takedowns, trademark and counterfeit enforcement, doxxing and privacy removals, revenge-porn and deepfake (NCII) takedowns, and CSAM reporting. Packaged as an [agent skill](https://docs.claude.com/en/docs/claude-code/skills) for Claude Code and the Agent SDK.

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Type: Agent Skill](https://img.shields.io/badge/type-agent%20skill-8A2BE2)
![Scope: Global](https://img.shields.io/badge/scope-global-brightgreen)
![Last fact-check: Jul 2026](https://img.shields.io/badge/fact--checked-Jul%202026-orange)
![PRs: Welcome](https://img.shields.io/badge/PRs-welcome-success)

Most takedowns fail for two boring reasons. The requester tips off the operator before freezing evidence, or signs the notice in their own name and publishes it to a public database. This skill encodes the discipline that avoids both, plus the channel, legal basis, and escalation path for each content type, so a person or an AI agent can act correctly under pressure.

## TL;DR
A step-by-step checklist for getting stolen, fake, or harmful content off the internet: who hosts it, who to contact, what to say, and in what order. Read it yourself, or install it into an AI assistant (Claude Code) and just describe your problem in plain words.

## Table of contents
- [TL;DR](#tldr)
- [Why this exists](#why-this-exists)
- [What it covers](#what-it-covers)
- [The five invariants](#the-five-invariants)
- [Quick start](#quick-start)
- [How it works](#how-it-works)
- [Who it's for](#who-its-for)
- [Legal and jurisdiction coverage](#legal-and-jurisdiction-coverage)
- [Maintenance](#maintenance)
- [Disclaimer](#disclaimer)
- [Contributing](#contributing)
- [License](#license)

## Why this exists
Removal knowledge is scattered across host AUPs, registrar policies, search-engine forms, and a dozen legal regimes that change every year. Generic advice sends a DMCA to Cloudflare, which never hosts anything. It runs WHOIS on a domain where WHOIS was sunset. It tells a doxxing victim to file in their own name. This skill puts the correct path (recon, then evidence, then the right channel, then escalation) into one place an agent can load and act on.

## What it covers

| If someone is… | Route |
|---|---|
| Pirating your images, video, course, or code | Copyright / DMCA §512 to the origin host plus search de-indexing |
| Impersonating your brand or selling counterfeits | Trademark AUP and brand programs plus UDRP or URS (domain disputes) |
| Posting your address, phone, or private data | Doxxing / privacy removal plus Google "Results about you" |
| Sharing intimate images or AI deepfakes without consent | NCII via StopNCII and the US TAKE IT DOWN Act (48-hour removal) |
| Hosting content that sexualizes minors | CSAM reporting to authorities. Never collect it yourself |
| Defaming you | The court-order reality, plus voluntary-removal levers |

## The five invariants
These apply to every takedown, whatever the content type:
1. Freeze evidence before the first notice: SHA-256 byte-identity, Wayback, archive.today, Perma.cc, and court-grade options.
2. Protect the requester's identity. Notices get republished on the public Lumen Database, so file as an authorized agent.
3. Prove file identity, not similarity. A matching hash is dispositive.
4. Never collect illegal content. Report the URL and the site's own label.
5. A CDN or registrar is not the host. Only the origin host removes files.

## Quick start

Install as a Claude Code plugin (recommended):
```
/plugin marketplace add goul4rt/content-takedown
/plugin install content-takedown@goul4rt
```

Or install as a personal skill by cloning the skill folder directly:
```bash
git clone https://github.com/goul4rt/content-takedown.git /tmp/ct \
  && cp -r /tmp/ct/skills/content-takedown ~/.claude/skills/content-takedown
```

The skill auto-activates when a conversation matches its description (takedown, DMCA, counter-notice, delisting, revenge porn, deepfake removal, cybersquatting/UDRP, brand abuse). In Claude Code you can also invoke it explicitly with `/content-takedown`.

## How it works
The skill is built on progressive disclosure so it stays fast and cheap to load:
```
content-takedown/                       # repo = Claude Code plugin + marketplace
├── .claude-plugin/
│   ├── plugin.json                     # plugin manifest
│   └── marketplace.json                # marketplace manifest
└── skills/content-takedown/
    ├── SKILL.md                        # always loaded: invariants, decision flow, routing table
    └── references/
        ├── legal-regimes.md            # DMCA, DSA, TAKE IT DOWN, STF Tema 987, ECA Digital, UK OSA, LCEN/SREN, DDG, RTBF
        ├── forms-directory.md          # form and channel URLs (verify at runtime)
        └── recon-fingerprints.md       # RDAP by TLD, CDN/host header fingerprints, abuse discovery
```
Stable rules live in `SKILL.md`. Fast-moving facts like fees, form URLs, and trusted-flagger lists live in the reference files, flagged "verify at runtime" instead of hardcoded, so the guidance does not rot.

## Who it's for
- Lawyers and brand-protection teams running removals at scale.
- Creators and small businesses hit by piracy, impersonation, or counterfeit stores.
- Individuals facing doxxing, revenge porn, or deepfakes.
- AI agents and automation that need a correct, auditable takedown procedure.

## Legal and jurisdiction coverage
US DMCA §512, US TAKE IT DOWN Act, EU DSA (Art. 16 and trusted flaggers), EU GDPR right to be forgotten, France LCEN/SREN, Germany DDG, UK Online Safety Act, Brazil Marco Civil (after STF Tema 987), ECA Digital, Lei 9.610, and WIPO UDRP/URS for cybersquatting. The NO FAKES Act is tracked as watch-only until enacted.

## Maintenance
Re-verify quarterly: WIPO/URS fees, the DSA trusted-flagger list, the Lumen contributor list, per-ccTLD RDAP coverage, and any NO FAKES Act enactment. Last fact-check: July 2026.

## Disclaimer
This is an operational runbook, not legal advice. Laws, fees, and forms change. Confirm current facts and consult a qualified lawyer in your jurisdiction before acting.

## Contributing
Corrections to channels, form URLs, fees, or legal regimes are welcome. Open an issue or a PR. Keep entries specific (name the form, the article, the timeline) and flag anything you could not verify.

## License
MIT, see [LICENSE](LICENSE).

---

<sub>Keywords: DMCA takedown notice, remove content from Google, content removal playbook, revenge porn removal, deepfake and NCII takedown, TAKE IT DOWN Act, doxxing removal, counterfeit and brand protection, UDRP cybersquatting, CSAM reporting, RDAP and WHOIS recon, Cloudflare origin host, Claude Code skill, AI agent skill.</sub>
