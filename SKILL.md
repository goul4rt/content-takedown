---
name: content-takedown
description: Use when getting online content removed — copyright/DMCA, trademark/impersonation/counterfeit, illegal content (CSAM), or privacy/doxxing/non-consensual imagery/defamation — including recon (registrar/host/CDN), which channel and legal regime applies, and protecting the requester's identity from public disclosure.
---

# Content Takedown Runbook

## Overview

Getting content off the internet means notifying the right infrastructure or platform layer, with the right legal basis, in the right order. The type of content determines the channel; the mechanics of recon, evidence, and identity-protection are the same every time.

**Five invariants apply to EVERY takedown, regardless of type. Agents reliably skip them under "just take it down fast" pressure. Do not.**

## The five invariants

### 1. Freeze evidence BEFORE the first notice
Your first notice tips off the operator, who can wipe or alter the content. Capture first:
- **Byte-identity proof:** `curl -s <infringing-url> | shasum -a 256` and compare to your original's hash. A matching SHA-256 = **literally the same file**, not "similar" — the single strongest exhibit.
- **Neutral third-party preservation:** submit URLs to `web.archive.org/save/` and `archive.today`. Capture `curl -sI` headers and `dig`/RDAP output (they change).
- **Timestamped screenshots** with URL bar + date visible.
- ⚠️ **Exception — never applies to illegal content you cannot lawfully possess** (see type 3).

### 2. Protect the requester's identity (Lumen exposure)
Google, Cloudflare, X, WordPress and others forward notices **verbatim to the public Lumen Database (lumendatabase.org)** — the sender's name, address, and email become permanently searchable. This is acute for **doxxing/harassment victims** (a DMCA on your own stolen photos re-publishes your name) and for brands not wanting to signal enforcement.
- File **as an authorized agent** ("acting on behalf of the rights holder / affected party"). Both DMCA §512(c)(3) and DSA Art. 16 explicitly permit an agent to sign. The agent's name appears, not the client's.
- Use the **attorney's/agent's** name, a professional address, and a **role email** — never the client's personal name/email.
- **Assume every form field may be published.** Put nothing anywhere you would not publish.

### 3. Prove identity of the file, don't assert similarity
"Their logo looks like mine" is weak. Matching SHA-256 + identical byte count is dispositive. Use it whenever the copy is a served file (image, video, PDF, asset).

### 4. Never collect illegal content
For suspected CSAM or anything whose mere possession is a crime: **do NOT download, save, hash-yourself, or screenshot it.** Report only the **URL + the site's own text label**, and hand to authorities who are legally authorized to retrieve it. Frame as measured fact ("the site's own taxonomy labels this X; this potentially constitutes Y; referred for verification") — never assert as verified what you must not verify.

### 5. A CDN/registrar is not the host
A CDN (Cloudflare, Akamai, Fastly, Bunny, Sucuri, CloudFront) is a **pass-through proxy** — it forwards your notice and may disclose the origin, but **does not remove content**. A registrar controls the **name**, not the content. Chase the disclosed **origin host** — that is the only layer that removes files. Note: Cloudflare Email Routing (MX = `*.mx.cloudflare.net`) is **receive-only** — you cannot send from a domain that uses it; send from real SMTP or the agent's own mailbox.

## Step 1 — Recon (works for any domain)

```bash
# Registration data — RDAP is now authoritative (ICANN sunset WHOIS Jan 2025).
# whois returning blank on a new gTLD (.app/.shop/.dev) is EXPECTED, not an error.
curl -s https://rdap.org/domain/<domain> | jq   # registrar, dates, abuse contact
# ccTLDs (.fr/.de/.uk/.br) may be WHOIS-only or use the registry's own lookup.

dig +short <domain>                              # A record → IP
whois <IP>    # or: curl -s https://rdap.org/ip/<IP> | jq   → network owner = host or CDN
curl -sI https://<domain>/ | grep -iE 'server|via|cf-ray|x-amz-cf-id|x-served-by|x-akamai'
crt.sh: https://crt.sh/?q=%25.<domain>           # subdomains / sometimes origin hostname
```
Fingerprints: `CF-RAY`=Cloudflare · `X-Amz-Cf-Id`=CloudFront/AWS · `X-Served-By`+`Via: varnish`=Fastly · `AkamaiGHost`=Akamai · `Sucuri/Cloudproxy`=Sucuri · `BunnyCDN`=Bunny. If the IP resolves straight to a host (no CDN), that IP **is** the origin — skip the CDN chase.

Find a US host's DMCA agent: `copyright.gov/dmca-directory/`. First abuse probe: `abuse@<domain>` (RFC 2142); also abuse.net, AbuseIPDB, the RIR's `abuse-c`.

## Step 2 — Route by content type

| Type | Primary channel | Special rules |
|---|---|---|
| **Copyright** | DMCA §512(c) to host/DMCA-agent; §512(d) to search engines; forward via CDN if origin hidden | Six required elements (below). **§512(f):** don't overclaim — claiming works you don't own or fair uses = liability. |
| **Trademark / impersonation / counterfeit** | Host AUP (NOT DMCA — copyright ≠ trademark) + platform brand programs (Amazon Brand Registry, eBay VeRO, Meta/Google TM). For the domain itself → **UDRP** | UDRP (WIPO ~$1,500, ~45–60d): prove all 3 — identical/confusingly similar + no legitimate interest + bad faith. Remedy = transfer. **URS** (~$300–500, ~2–3wk): clear-cut new-gTLD abuse, suspension only. |
| **Illegal / CSAM** | **Authorities, not takedown forms.** NCMEC CyberTipline (`report.cybertip.org`), INHOPE hotline (`inhope.org`), IWF UK, PHAROS FR, SaferNet BR | **Invariant 4 — never collect.** Keep separate from any copyright claim. Take It Down (`takeitdown.ncmec.org`) for intimate imagery of a minor. |
| **Privacy / doxxing / NCII (adult)** | Google PII/doxxing removal (`support.google.com/websearch/answer/9673730`) + **StopNCII.org** (on-device hash, image never leaves device) | Search removal ≠ source removal — do both. EU: RTBF (GDPR Art. 17). **Invariant 2 is critical here** — don't re-expose the victim on Lumen. |
| **Defamation** | Usually **requires a court order** — platforms rarely remove on notice; US §230 immunizes them; BR retains judicial-order rule for honor crimes | Get the order first, then present to host/search engine. |

## Step 3 — Channel order & escalation

**Order:** freeze evidence → registrar/registrant abuse → CDN (to reveal origin) → **origin host (removal)** → search de-index → platform/payment/app-store chokepoints → authorities (if illegal) → escalate.

**Escalation ladder if ignored:** CDN → host/DMCA-agent → registrar abuse → registry abuse → **ICANN compliance** (contract failures only, e.g. dead abuse contact — not content disputes) and/or UDRP/URS for the name.

**Beyond the host** (starve/hide the operation): search de-indexing (Google/Bing removal forms); payment processors (Stripe/PayPal/CCBill abuse — often ends a store faster than a host); ad networks (Google Ads/AdSense); app stores (Apple/Google Play IP forms); social-platform impersonation/IP flows.

Keep a **tracker**: one row per channel — target, date, ticket/protocol #, status. **Screenshot before and after every submission.**

## Legal regimes (which bites where)

- **US — DMCA §512.** §512(c)(3) notice needs six elements: (1) signature; (2) the copyrighted work; (3) the infringing material + locating info (URLs); (4) contact info; (5) good-faith-belief statement; (6) accuracy + authority under penalty of perjury. **§512(g) counter-notice** forces restore in **10–14 business days** unless you file suit — a takedown can commit you to litigation.
- **EU — DSA (Reg. 2022/2065) Art. 16** notice-and-action for all "illegal content": explain why illegal + exact URL + notifier name/email (waived for CSAM) + good-faith statement. Trusted-flagger notices (Art. 22) are prioritized.
- **France — LCEN Art. 6** host liability on knowledge; formal notification per Art. 6-I-5. PHAROS = `internet-signalement.gouv.fr`.
- **Germany — DSA/DDG** (NetzDG largely repealed May 2024 — route via DSA Art. 16).
- **Brazil — Marco Civil (Lei 12.965).** ⚠️ **STF Tema 987 (Jun 2025)** made notice-and-takedown the general rule (extrajudicial notice now creates liability for most illegal content); serious crimes → remove immediately; **defamation/honor crimes still need a court order**; NCII (Art. 21) removable on simple notice. Copyright = Lei 9.610/98; CSAM = ECA Art. 241.
- **UK — Online Safety Act (Ofcom)** is systemic compliance, not per-item notice.
- Many hosts accept a DMCA-style notice globally regardless of jurisdiction. A US DMCA has no force on a non-US host that never claimed §512 — use DSA/local law there.

## Common mistakes

| Mistake | Fix |
|---|---|
| Signing in the client's own name | File as authorized agent; assume Lumen publishes every field (esp. doxxing victims) |
| "whois failed" on a new gTLD | Use RDAP — whois is expected to be blank; ccTLDs use their own registry |
| DMCA to Cloudflare expecting removal | CDN only forwards + reveals origin; DMCA the real host |
| Using DMCA for a trademark/counterfeit problem | Copyright ≠ trademark — use host AUP + brand programs + UDRP |
| Collecting/screenshotting suspected CSAM | Report URL + label only to authorities; never possess it |
| "Their logo looks like mine" | Prove byte identity with matching SHA-256 |
| Filing before freezing evidence | Notice tips off the operator; capture SHA-256 + archive first |
| Overbroad claim (works you don't own / fair use) | §512(f) liability — scope to what you can substantiate |
| Treating a CDN/registrar ack as a takedown | Only the origin host removes content; chase it |

## Verify at runtime
Forms, fees, and legal contours change. Re-check: WIPO/URS fees; Brazil STF Tema 987 legislative follow-up; LCEN prior-contact requirement; per-ccTLD RDAP availability; exact Google/platform form URLs (search the phrasing if a link moved).
