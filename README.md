# Virgin Australia (virgin-australia)

Virgin Australia (IATA code VA, ASX:VGN) is Australia's second-largest airline group, founded in 2000 and headquartered in Brisbane, operating a domestic and short-haul international network alongside charter and cargo services with more than 8,000 team members and the 13-million-member Velocity Frequent Flyer loyalty program. It sits on the airline side of a concentrated Australian duopoly with Qantas, and reaches travel agents and travel management companies almost entirely through GDS intermediation — Amadeus, Sabre and Travelport/Galileo — rather than through any direct machine interface of its own. Virgin Australia selected Sabre as its preferred NDC IT technology provider in October 2023 and renewed a multi-year Amadeus distribution agreement covering EDIFACT today and NDC in future, but as of July 2026 no Virgin Australia NDC endpoint, developer portal or API documentation is published anywhere on virginaustralia.com. Its API posture is honestly stated as none-published.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/virgin-australia/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/virgin-australia/refs/heads/main/apis.yml)

## Tags

- Travel
- Australia
- Aviation
- Airline
- Distribution
- GDS
- NDC
- Booking
- Loyalty
- Corporate Travel

## Timestamps

- **Created:** 2026-07-28
- **Modified:** 2026-07-28

## APIs

None. Virgin Australia publishes no developer portal, no API reference, and no machine-readable API contract of any kind.

Every candidate host and path was probed on 2026-07-28:

| Surface | Status |
| --- | --- |
| `developer.virginaustralia.com` | DNS does not resolve |
| `developers.virginaustralia.com` | DNS does not resolve |
| `api.virginaustralia.com` | DNS does not resolve |
| `apis.virginaustralia.com` | DNS does not resolve |
| `docs.virginaustralia.com` | DNS does not resolve |
| `portal.virginaustralia.com` | 200 — F5 BIG-IP corporate SSO gateway, not a developer portal |
| `/developers`, `/api`, `/api-docs` | 404 |
| `/openapi.json`, `/swagger.json` | 404 |
| `/.well-known/security.txt`, `/.well-known/ai-plugin.json`, `/llms.txt` | 404 |
| `github.com/virginaustralia` | 404 — no GitHub organization |

The `robots.txt` and the fully enumerated AU sitemap contain zero developer or `/api` URLs.

## Two Surfaces

**Consumer surface** — no public API. Booking, check-in, flight status and Velocity loyalty run through virginaustralia.com, the mobile apps, and (since 10 June 2026, an Australian airline first) the Virgin Australia app in ChatGPT, which searches cash fares, Points + Pay and Velocity Reward Seats but by its own terms does "not process payments; create, amend or cancel bookings; confirm final pricing, fare availability or fare conditions; or make binding commitments." `velocityfrequentflyer.com/robots.txt` disallows `/api/*` and `/data/*`, confirming an internal-only, undocumented JSON surface.

**Distribution surface** — GDS-intermediated. The actual integration contract with travel agents is GDS cryptic-entry syntax, published in the agent FAQ:

- Amadeus — `OS VA VACC/insert company's ABN`
- Galileo — `SI.VAA*VACC/insert company's ABN`
- Sabre — `30SI VA VACC/insert company's ABN`
- Plus: "Account code ACC99 must be included on every ticket to ensure accurate reporting."

Three non-interchangeable dialects wrapping the same semantics. Duffel, which resells Virgin Australia, states plainly: "Currently, we access this airline's content through Travelport, a global distribution system designed for the travel industry."

## NDC Posture

Committed, not shipped. Sabre was named preferred NDC IT technology provider on 31 October 2023 to power a "future" NDC connection, and the Amadeus agreement covers agency content via EDIFACT "and, in the future, NDC channels." No Virgin Australia NDC endpoint, schema version, IATA NDC certification level or Airline Retailing Maturity index entry was found. No GDS or distribution surcharge is published — the OB Ticket Fee on 795 ticket stock is a card payment surcharge, not a channel fee.

## What It Costs to Leave

- **Interface shape:** proprietary-undocumented — nothing published to swap out, and nothing standard to swap in.
- **Second source:** none. You can change GDS (Amadeus ↔ Sabre ↔ Travelport) but you cannot buy a Virgin Australia seat from anyone but Virgin Australia. Codeshare partners, Duffel and other aggregators are thinner wrappers over the same GDS dependency.
- **Exit path:** no export published. Access and correction rights exist under the Privacy Act 1988 (Cth); a genuine portability right ("transfer your personal information to a third party in a portable format") is offered only to EEA and UK residents. No mechanism, format or endpoint for either.
- **Identifiers:** portable ones (VA, VOZ, 795 plating code, IATA/ARC agency numbers, IATA airport codes, PNR record locators, BSP identifiers) sit alongside non-portable ones (the Virgin Australia Code, account code ACC99, the VACC OSI keyword, and Velocity membership numbers and Points — a closed-loop currency with no export route).
- **Contract:** the Travel Agent Main Agreement requires "current IATA or ATIS accreditation" and states "You must have GDS access for BSP Sales" — the airline contractually *mandates* intermediation. Virgin Australia may refuse an account "in our absolute discretion," may run a third-party credit assessment, and may remove ticketing authority where "your Account is inactive or not utilised for at least 60 days." No exclusivity clause, full-content agreement, minimum volume or segment fee was found in the published agreement.
- **Access gate:** accredited-or-licensed. There is no developer tier at all — an agent needs IATA/ATIS/ARC accreditation, a GDS PCC, a credit check, and discretionary approval.

## The Seam

In the Australian market the switching-cost story does not run through the airlines. Virgin Australia and Qantas are a duopoly on inventory nobody else can supply, and neither publishes an interface, so there is nothing to lock into beyond the seats. It runs through hospitality: SiteMinder, built in Sydney, publishes five documented integration APIs on OpenTravel schemas and occupies between hotels and every OTA exactly the position the GDS holds in aviation. The difference is that SiteMinder's lock-in is architectural and inspectable; Virgin Australia's is contractual and invisible.

## Artifacts

Because there is no specification to derive from, this repo carries only what could be
probed or honestly generated. Each file records verified absence as data, with HTTP status.

| Artifact | What it records |
| --- | --- |
| `well-known/virgin-australia-well-known.yml` | Every `/.well-known/` document (security.txt, openid-configuration, oauth-authorization-server, oauth-protected-resource, api-catalog, ai-plugin.json) plus `/llms.txt`, probed across all four reachable hosts on 2026-07-28. All 404. `trust.`, `security.`, `help.` and `status.virginaustralia.com` are NXDOMAIN. No `WellKnown` pointer is wired, because nothing is there. |
| `conformance/virgin-australia-conformance.yml` | Standards posture. Virgin Australia *consumes* IATA EDIFACT, BSP and industry codes through the GDS; it *publishes* no interface standard. NDC is announced via Sabre, not shipped. OpenAPI, AsyncAPI, GraphQL, OAuth 2.0, OIDC, RFC 9457, RFC 9116 and RFC 9727 all absent. No compliance certification published, so no `Compliance` pointer. |
| `security/virgin-australia-domain-security.yml` | TLS, HSTS, DNSSEC, CAA, SPF and DMARC for `www.virginaustralia.com`, `www.velocityfrequentflyer.com` and `api.velocityfrequentflyer.com`. Both domains: DMARC `p=reject`, SPF present, no DNSSEC, no CAA. `api.velocityfrequentflyer.com` serves TLS with HSTS but 404s everywhere. |
| `llms/virgin-australia-llms.txt` | Generated (Virgin Australia's own `/llms.txt` is 404 on every host) so an agent reading this company gets the honest answer — no API — instead of hallucinating one. |

Deliberately **not** created, because the underlying thing does not exist: `openapi/`, `asyncapi/`,
`graphql/`, `packages/` (no first-party library on npm, PyPI or any registry; no GitHub org),
`mcp/`, `skills/`, `scopes/`, `authentication/`, `errors/`, `overlays/`, `data-model/`,
`sandbox/`, `cli/`, `components/`, `changelog/`, `lifecycle/`, `grpc/`,
`security/virgin-australia-vulnerability-disclosure.yml` and `security/virgin-australia-trust-center.yml`.

## Maintainers

- Kin Lane — kin@apievangelist.com
