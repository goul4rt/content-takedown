# content-takedown

A vendor-neutral, global **content-takedown runbook** packaged as an agent skill (Claude Code / Agent SDK compatible). It encodes how to get online content removed — and how to do it without the two mistakes that quietly wreck real takedowns: **tipping off the operator before evidence is frozen**, and **publishing the requester's own name to a public database**.

It is deliberately generic. The same recon, evidence, and identity-protection mechanics apply whether the content is a pirated course, a counterfeit storefront, a doxxing page, or an impersonation site — only the channel and legal basis change.

## What it covers

- **Copyright / DMCA** — §512(c) notices, the six required elements, §512(f) over-claim risk, §512(g) counter-notice window.
- **Trademark / impersonation / counterfeit** — host AUP + brand programs (Amazon Brand Registry, eBay VeRO, …), UDRP/URS for the domain itself.
- **Illegal content (CSAM)** — routing to authorities (NCMEC, INHOPE, IWF, PHAROS, SaferNet) with a hard rule: **never download or collect the material**.
- **Privacy / doxxing / non-consensual intimate imagery / defamation** — Google PII & NCII removal, StopNCII / Take It Down, GDPR right-to-be-forgotten, and when a court order is unavoidable.

## The five invariants

Every takedown, regardless of type:

1. **Freeze evidence before the first notice** — SHA-256 byte-identity proof, Wayback/archive.today, timestamped captures.
2. **Protect the requester's identity** — notices are republished verbatim on the public [Lumen Database](https://lumendatabase.org); file through an authorized agent so the client's name never appears.
3. **Prove file identity, not similarity** — a matching SHA-256 is dispositive; "looks like mine" is not.
4. **Never collect illegal content** — report the URL and the site's own label; let authorities retrieve it.
5. **A CDN/registrar is not the host** — a proxy forwards and may reveal the origin, but only the origin host removes files.

## Recon & legal-regime coverage

- **RDAP-first recon** (ICANN sunset WHOIS in Jan 2025), CDN fingerprinting (Cloudflare, Akamai, Fastly, Bunny, Sucuri, CloudFront), certificate-transparency pivoting, DMCA-agent lookup.
- **Legal regimes** with article numbers, timelines, and fees: US DMCA §512, EU DSA Art. 16/22, France LCEN Art. 6, Germany DSA/DDG, Brazil Marco Civil (incl. the June 2025 STF *Tema 987* ruling), UK Online Safety Act, and WIPO UDRP/URS.

## Install

Clone into your agent's skills directory:

```bash
git clone https://github.com/goul4rt/content-takedown.git ~/.claude/skills/content-takedown
```

The skill activates automatically when a conversation matches its description. You can also invoke it explicitly (e.g. `/content-takedown` in Claude Code).

## Structure

```
content-takedown/
  SKILL.md     # the skill itself (invariants, recon, routing matrix, regimes)
  README.md    # this file
  LICENSE
```

## Disclaimer

This is an **operational runbook, not legal advice.** Forms, fees, and legal contours change — the skill's "Verify at runtime" section flags the fast-moving pieces (WIPO fees, Brazil's STF follow-up legislation, per-ccTLD RDAP availability, exact form URLs). Confirm current facts and consult a qualified lawyer before acting in your jurisdiction.

## Contributing

Corrections to channels, form URLs, fees, or legal regimes are welcome — open an issue or PR. Keep entries specific (name the form, the article number, the timeline) and flag anything you could not verify.

## License

MIT — see [LICENSE](LICENSE).
