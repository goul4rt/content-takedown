---
name: content-takedown
description: "Use when getting online content removed or de-indexed, across copyright/DMCA, trademark/impersonation/counterfeit, illegal content (CSAM), privacy/doxxing/defamation, or non-consensual intimate imagery including AI-generated deepfakes/NCII. Covers recon (registrar/host/CDN), which channel and legal regime applies, evidence preservation, and protecting the requester's identity from public disclosure. Trigger on takedown, DMCA, counter-notice, delisting, revenge porn, deepfake removal, cybersquatting/UDRP, brand abuse, or 'get this off the internet'."
---

# Content Takedown Runbook

## Overview
Getting content off the internet means notifying the right infrastructure or platform layer, with the right legal basis, in the right order. Content type determines the channel. The recon, evidence, and identity-protection mechanics are the same every time. Keep this file loaded; read the reference files only for the regime or form you actually need.

## Five invariants (apply to EVERY takedown)
1. **Freeze evidence BEFORE the first notice.** Capture SHA-256 byte-identity (`curl -sL URL | shasum -a 256`), archive to web.archive.org/save and archive.today (add Perma.cc for litigation-grade; in Brazil use ata notarial or e-Not Provas via cartório for court-grade proof), save `curl -sI` headers, dig/RDAP output, and timestamped screenshots. Reason: hosts remove content within hours and unpreserved evidence is unrecoverable. EXCEPTION: never for illegal content (invariant 4).
2. **Protect the requester's identity.** Notices are forwarded to Lumen (lumendatabase.org) by Google, Meta, GitHub, Reddit, Wikipedia and others. The public view redacts PII by default, but assume every field may surface. File as authorized agent under DMCA §512(c)(3) and DSA Art. 16; use the attorney or agent name, a professional address, and a role email, never the client's personal identity. If a name leaks, request Lumen redaction (case-by-case).
3. **Prove file identity, not similarity.** Match SHA-256 (or the platform's perceptual hash for NCII). Never assert "looks the same"; assert byte or hash identity.
4. **Never collect illegal content.** For CSAM: no download, save, hash, or screenshot. Report the URL plus the site's own label to authorities (NCMEC CyberTipline, INHOPE, IWF, PHAROS, SaferNet BR). Take It Down (NCMEC) for minors' intimate imagery; StopNCII for adults.
5. **A CDN/registrar is not the host.** CDNs are pass-through; registrars control the name; only the origin host removes files. Do not send removal demands to a CDN expecting deletion (it forwards and notifies only). Cloudflare Email Routing MX is receive-only, not a hosting signal.

## Step 1: Recon
See `references/recon-fingerprints.md`. Fast path: RDAP via rdap.org (ICANN sunset WHOIS 28 Jan 2025; blank data on new gTLDs is expected; many ccTLDs are RDAP-now, e.g. rdap.registro.br for .br). Run `dig +short`, `whois <IP>`, then `curl -sI` header fingerprints to tell a CDN from the origin. Use `crt.sh` for subdomains and origin leaks. DMCA agent lookup at copyright.gov/dmca-directory. Abuse contacts: abuse@domain (RFC 2142), abuse.net, AbuseIPDB, RIR abuse-c.

## Step 2: Route by content type

| Content type | Primary channel | Legal basis | Notes |
|---|---|---|---|
| Copyright | §512(c) notice to host/agent; §512(d) to search engines; forward via CDN | US DMCA §512 (six elements; §512(f) overclaim liability) | A non-US host that never claimed §512 owes no DMCA duty. Use local law or the host AUP |
| Trademark / impersonation / counterfeit | Host AUP + brand programs (Amazon Brand Registry, eBay VeRO, Meta Brand Rights Protection, Google/TikTok/Shopify IP) + UDRP or URS | Not DMCA | UDRP: transfer remedy. URS: suspension only. Fees and timeline: verify at runtime |
| Illegal / CSAM | Authorities, NOT forms | ECA art. 241 (BR); national criminal law | Never collect (invariant 4). Report URL + site label only |
| NCII / deepfake (adult) | StopNCII.org (on-device hash) + platform NCII flow + US TAKE IT DOWN notice (48h) | US TAKE IT DOWN Act (in force); BR MCI art. 21; state laws | Covers AI "digital forgeries" of real people |
| NCII (minor) | NCMEC Take It Down + CyberTipline | Federal criminal / ECA | Treat as illegal content |
| Privacy / doxxing | Google "Results about you" + PII removal form; source-host removal | GDPR Art. 17 (EU RTBF); local privacy law | Search removal is not source removal |
| Defamation | Usually a court order | US §230; BR: honor crimes still need a court order (STF Tema 987) | Notice can still prompt voluntary removal |

## Step 3: Channel order and escalation
Freeze evidence, then registrar/registrant abuse, then CDN (to reveal or notify the origin), then origin host (removal), then search de-index, then platform/payment/app-store chokepoints, then authorities, then escalate.
Escalation ladder: CDN, host, registrar abuse, registry abuse, ICANN compliance (contract failures ONLY), and/or UDRP/URS. Beyond the host: search de-indexing, payment processors (Stripe/PayPal/CCBill abuse), ad networks (Google Ads abuse), app stores (Apple App Store dispute, Google Play IP form), social impersonation flows. Keep a tracker; screenshot before and after every submission.

## Defense side (client RECEIVES a takedown)
If your client is the target: do not delete their local copies. For DMCA, a §512(g) counter-notice (10 to 14 business-day window) restores content if the claimant does not sue. It also exposes the counter-notifier's identity and consents to jurisdiction, so advise accordingly. Note that §512(f) liability runs against knowingly false notices, which gives leverage against abusive claims. For TAKE IT DOWN and NCII removals there is no robust statutory counter-notice; dispute through the platform's appeal channel.

## Common mistakes → fix
- Signing in the client's own name → file as agent.
- WHOIS blank on a new gTLD → use RDAP.
- DMCA to Cloudflare expecting removal → DMCA the real host; only notify or forward via the CDN.
- DMCA for a trademark problem → AUP + brand programs + UDRP/URS.
- Collecting CSAM → report URL + label only.
- Asserting similarity → prove SHA-256/hash identity.
- Filing before freezing evidence → freeze first.
- Overbroad copyright claims → §512(f) exposure.
- Treating a CDN/registrar acknowledgement as a takedown.

## Verify at runtime (do not hardcode)
WIPO/URS fees and timelines; exact form URLs (`references/forms-directory.md`); per-ccTLD RDAP coverage; the DSA trusted-flagger list; the LCEN prior-contact requirement; the current Lumen contributor list; any NO FAKES Act enactment.
