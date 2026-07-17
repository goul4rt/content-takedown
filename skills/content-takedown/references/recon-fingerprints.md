# Recon & fingerprints

## RDAP / WHOIS
- WHOIS sunset by ICANN: 28 Jan 2025. RDAP overtook WHOIS in volume mid-2025.
- Query rdap.org (it redirects to the authoritative server). All gTLDs are on RDAP; ccTLDs are partial.
- .br → rdap.registro.br (owner data semi-public; CPF/CNPJ only with auth; about 5 queries/min/IP).
- .uk / .de / .fr → RDAP coverage varies; verify the endpoint per TLD. A blank or redacted registrant is expected post-GDPR/LGPD.

## CDN / host header fingerprints (indicative, not proof; headers are suppressible)
| Signal (response header) | Provider |
|---|---|
| CF-RAY, cf-cache-status | Cloudflare |
| X-Amz-Cf-Id, Via: ...cloudfront | Amazon CloudFront |
| X-Served-By + Via: varnish, Fastly-* | Fastly |
| AkamaiGHost, X-Akamai-* | Akamai |
| X-Sucuri-ID, X-Sucuri-Cache | Sucuri |
| Server: BunnyCDN | Bunny |
| x-vercel-id, Server: Vercel, x-vercel-cache | Vercel |
| x-nf-request-id, Server: Netlify, x-served-by: cache-...netlify | Netlify |
| Server: GitHub.com | GitHub Pages |
| x-iinfo, cookies visid_incap_/incap_ses_ | Imperva/Incapsula |
| fly-request-id, Server: Fly/fly.io | Fly.io |

## Origin discovery
- crt.sh for certificate transparency (subdomains, historical origin).
- Compare the origin IP against CDN ranges; check for direct-IP leaks, mail (MX) hosts, and old DNS records.
- Reminder: a CDN or registrar is not the host. Only the origin host removes files.

## Abuse discovery
- abuse@domain (RFC 2142); abuse.net; AbuseIPDB; RIR abuse-c (ARIN/RIPE/APNIC/LACNIC/AFRINIC).
