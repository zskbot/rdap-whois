# Security Center integration contract — rdap-whois

## Audit snapshot
- TypeScript ES module library, package name `@aoxborrow/rdap-whois`, Node.js engine `>=22.12`, MIT license in package metadata.
- README describes RDAP-first lookup with WHOIS fallback, normalized records, referrals, retries, rate limits, typed errors, CLI and Cloudflare Workers support.
- Package scripts include build, typecheck, lint, Vitest tests, browser/pack/example checks and registry health utilities.
- The README/package metadata identify the upstream project as `aoxborrow/rdap-whois`; confirm fork ownership and retain upstream attribution before publishing modified artifacts.

## Integration role
This is the domain-registration intelligence adapter. It complements DNS/SPF and phishing signals but is not itself a maliciousness verdict.

## Preview API contract
- `POST /v1/intel/domain/lookup` with a normalized domain (not a full arbitrary URL).
- Return registrar, registry, creation/expiration dates, nameservers, DNSSEC, EPP status, availability and source protocol/server type when present.
- Cache by normalized domain and respect registry/registrar rate limits and `Retry-After`.

## Guardrails
- Do not accept caller-supplied WHOIS/RDAP server URLs or proxy destinations.
- Bound concurrency, retries and total request deadline.
- Redact registrant personal data by default; only return fields required by the product.
- Handle missing RDAP/WHOIS service as unknown, not as a safe verdict.
- Use the existing library APIs; do not copy the library implementation into the gateway.
