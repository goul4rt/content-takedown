# content-takedown

A vendor-neutral, global runbook skill for getting online content removed or de-indexed:
copyright/DMCA, trademark/impersonation/counterfeit, illegal content (CSAM),
privacy/doxxing/defamation, and non-consensual intimate imagery including AI deepfakes/NCII.

## What it does
Guides recon (registrar/host/CDN), routing to the right channel and legal regime,
evidence preservation, requester-identity protection, and escalation — including the
defense side when a client receives a takedown.

## Structure (progressive disclosure)
- `SKILL.md` — five invariants, decision flow, routing table, common mistakes (always loaded).
- `references/legal-regimes.md` — DMCA, DSA, TAKE IT DOWN Act, STF Tema 987, ECA Digital, UK OSA, LCEN/SREN, DDG, RTBF.
- `references/forms-directory.md` — form/channel URLs (verify-at-runtime pattern).
- `references/recon-fingerprints.md` — RDAP by TLD, CDN/host header fingerprints, abuse discovery.

## Design principles
- Content type determines the channel; recon/evidence/identity mechanics are constant.
- Fast-moving facts (fees, form URLs, trusted-flagger lists) are NOT hardcoded — verify at runtime.
- Never collect illegal content; report URL + the site's own label to authorities.
- Preserve evidence before the first notice (SHA-256, archives, Perma.cc; ata notarial/e-Not Provas in Brazil).

## Install
```bash
git clone https://github.com/goul4rt/content-takedown.git ~/.claude/skills/content-takedown
```

## Maintenance
Re-verify quarterly: WIPO/URS fees, DSA trusted-flagger list, Lumen contributor list,
per-ccTLD RDAP coverage, and any NO FAKES Act enactment.
Last fact-check: July 2026.
