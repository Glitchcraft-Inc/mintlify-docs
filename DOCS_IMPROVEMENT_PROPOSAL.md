# Actioneer Docs: Improvement Proposal — Benchmarked Against Dodo Payments

## Executive Summary

The single biggest thing Dodo does that we don't: **a front door.** Dodo opens with a landing/introduction page that answers "what is this, is it for me, where do I start" in one screen, then fans out by reader intent into role-based tabs (Features / Developer Resources / API Reference). Actioneer has no landing page, no quickstart, and no top-nav tabs — a newcomer is dropped directly onto `integrations/data-warehouses` with zero orientation, and the docs cover only two of the platform's surfaces (integrations + ingestion) while the differentiated agents/text-to-SQL product CLAUDE.md describes is entirely absent from the IA.

The through-line of this proposal: **our per-page craft already beats Dodo's** (rich frontmatter, the bespoke Mineral Trust CSS, the "At a glance" + Steps + AccordionGroup template, hype-free voice). What we lack is **structure above the page and trust surfaces around it** — an entry point, an intent-routing IA, a thin concept layer, forward cross-linking, an error reference, and the trust/retention extras (changelog, support, feedback) that signal an enterprise product is alive and maintained. The recommendations below add that scaffolding **without importing Dodo's marketing-heavy, breadth-for-breadth's-sake instincts**, which are off-brand for a governance-first enterprise product.

---

## 1. What Dodo Does Well (Transferable Principles)

### Onboarding & entry
- **A true landing page that triages by intent.** `/introduction` gives a one-sentence "what is this," self-qualify value cards, and a 3-step Quick Start that branches by persona (Humans vs AI Agents) and effort (No-Code → Full SDK). No reader has to guess where to start.
- **Goal → prerequisites → steps shape on every guide.** Guides open with a one-line goal and a plain prerequisites list before any code (e.g. `developer-resources/integration-guide`).
- **A sandbox-first path to first success.** `miscellaneous/test-mode-vs-live-mode` lets a user reach a working integration safely before production/verification.

### Information architecture
- **Role/altitude tabs keep every tree shallow.** The same surface is documented three times at three altitudes — `features/subscriptions` (concept) vs `developer-resources/subscription-integration-guide` (how-to) vs `api-reference/subscriptions` (reference) — so each reader self-selects.
- **Concept pages are hubs, not dead ends.** Nearly every page closes with a "Next Steps" `<CardGroup>` routing to the matching guide and reference (e.g. `features/usage-based-billing`).
- **Low-frequency-but-load-bearing content gets its own home.** Changelog and Miscellaneous (test-vs-live, accepted countries, policies, FAQs) are top-level, so they never pollute the product/reference trees.

### Content depth
- **Three distinct content types per feature** — concept, how-to, reference — cross-linked, never blended.
- **Concept pages establish vocabulary first:** plain-English definition → "How It Works" Steps → "Core Concepts" nouns, before any code.
- **Use-case recipes as a first-class genre** (Billing Deconstructions of real pricing models) and a **categorized error/troubleshooting reference** (`api-reference/error-codes`: code → trigger → message).

### API & developer experience
- **A narrative API Reference Introduction** front-loads the cross-cutting contract once: auth scheme, test/live base URLs, a rate-limit table with `X-RateLimit-*` headers, error shape.
- **OpenAPI-driven endpoint pages** with multi-language code tabs (cURL + ~10 SDK languages), each a complete copyable client call.
- **Webhook/reliability rigor:** signature verification, an explicit retry-backoff table, idempotency, timeouts.

### Visual polish
- **Every image in a `<Frame>`** (uniform borders, theme-aware chrome, captions) — the cheapest premium tell.
- **Screenshots/diagrams give even concept pages visual rhythm**; deliberate `<CardGroup>` icon grids turn lists into scannable menus; `<Tabs>` collapse breadth.

### Trust & retention
- **An always-current Changelog** in the top nav — the strongest single trust signal that the product ships.
- **A documented, tiered support model** with stated SLAs (instant AI → community → human 24–48h).
- **AI assistance as documented pages, not just chrome** (MCP server, agent skills) so it ranks and is evaluable.
- **One clear conversion path** and machine-readable `/llms.txt` + per-page `.md` exports for AI-agent discoverability.

---

## 2. Where Actioneer Stands Today

**Real strengths — we are ahead of Dodo on craft:**
- **Per-page SEO/OG hygiene is excellent.** Site-level `seo.metatags` (canonical `docs.actioneer.com`, og, twitter, `indexing: all`) plus full frontmatter on every page (`title`, `description`, `og:*`, `keywords[]`).
- **A distinctive visual identity Dodo lacks.** The Mineral Trust system (Newsreader headings, Inter body, Deep-Forest surfaces, mineral-green accents) pushes us past stock-Mintlify polish.
- **A genuinely strong how-to template.** `snowflake.mdx` = "At a glance" key-value table → least-privilege `<Info>` → tight `<Steps>` with `sql expandable` blocks → "Common questions" `<AccordionGroup>` → "Need a hand?" card. This is better than most of Dodo's guides.
- **Reliability docs at Dodo's webhook-grade level.** `ingestion/reliability.mdx` covers validation, the `{status:1}` acceptance contract, `event_id` idempotency/dedup, and a retry policy (5xx + network, exponential backoff with jitter, do-not-retry-4xx).
- **Clean section hubs.** `ingestion.mdx` and `data-warehouses.mdx` open with a hero, a one-line positioning statement, an "At a glance" table, and a `<CardGroup>` fanning to children.
- **On-brand voice throughout** — hype-free, governance-first, concrete.
- **AI/MCP chrome already enabled** via `contextual.options: ["copy","view","assistant","mcp","add-mcp"]` — Cmd+K search, Ask AI, and MCP export are on for free.

**Real gaps:**
- **No front door.** No landing page, no "what is Actioneer," no quickstart. First sidebar item is an integration overview.
- **No IA room to grow.** Two flat sidebar groups, `navbar.links` empty. The agents/text-to-SQL product — our differentiator — has no IA slot at all.
- **No concept layer.** Concepts (context layer, identity resolution, governance model) are smuggled into `<Info>` callouts inside how-tos.
- **Weak forward cross-linking.** Hub-and-spoke only; child pages (e.g. `snowflake.mdx`) end at the support card with no onward path.
- **Thin troubleshooting.** 3 accordion Q&As per page; no error-code catalog, no central FAQ. The SOC2/encryption answer appears copy-pasted across warehouse pages.
- **API/DX gaps.** No code-language tabs anywhere (raw HTTP/JSON only), no error/status reference, no base-URL/rate-limit table, and an **unverified "in-browser request builder"** promise in `api-reference.mdx`.
- **Trust surfaces nearly empty.** No changelog, no support/security page, no feedback widget, no analytics. The only CTA is a "Dashboard" button pointing at the **off-brand host `sentinel.gameramp.com`** — should be corrected regardless.
- **Visual texture is prose/table-heavy.** Only 3 of ~20 pages carry an image; heroes are bare `<img>` with hand-rolled Tailwind, not `<Frame>`; provider SVG logos sit unused in `images/` while cards fall back to generic Lucide icons; zero screenshots or data-flow diagrams.

---

## 3. Gap Analysis & Recommendations

Effort = S/M/L. Impact = Low/Med/High. **Brand-fit note in italics.**

### A. Onboarding & entry point

| # | Recommendation | What to do | Effort | Impact |
|---|---|---|---|---|
| A1 | **Add an Introduction landing page** | Create `introduction.mdx` (`mode:wide`), first entry in a new "Get Started" group. One-sentence definition (reuse `docs.json` description), short "What is Actioneer" para (CDP + agents + NL querying), a 3–4-card "Why Actioneer" `<CardGroup>` on the brand pillars (governance-first, operational continuity, read-only/auditable, benchmark leadership: DABstep/DA Bench/KRAMAbench), ending in cards routing to the two sections. | S | High |
| A2 | **Add a "Choose your path" decision block** | On the intro, a 2-card group: "Connect a data source" (Actioneer reads your warehouse in place, read-only) → `integrations/data-warehouses`; "Stream events in" → `ingestion`. Reuse the read-vs-push line already in `ingestion.mdx`. | S | High |
| A3 | **Add one canonical Quickstart** | `quickstart.mdx`: "At a glance" table (total time, prerequisites: workspace + admin on one source + egress IP `35.244.14.238`) → `<Steps>` (access workspace → connect first source → verify/ask first question) → "Next steps" cards. | M | High |
| A4 | **Add a "verify / first success" milestone** | Add a final "Confirm the connection" step to the quickstart and each provider page (run a sample read-only query / see tables appear). Optionally document pointing at a non-prod schema first. | M | Med |
| A5 | **Add an agent-first / MCP onboarding entry** | Short "Connect with your AI coding agent" card leveraging the already-enabled `add-mcp` option; one `<Info>` on governance framing. | S | Med |

*Brand fit: lead with concrete capability + benchmark proof, not hype. **Do NOT copy** Dodo's six-card "Why Choose Us" value-prop wall or "monetize in minutes" superlatives — keep our precise "~20 minutes" estimates.*

### B. Information architecture & navigation

| # | Recommendation | What to do | Effort | Impact |
|---|---|---|---|---|
| B1 | **Introduce top-nav tabs** | Restructure `navigation` into 2–4 tabs/anchors: Get Started, Integrations, Event Ingestion, and a forward-looking **Platform/Agents** tab. Keeps each sidebar tree shallow as content grows. | M | High |
| B2 | **Reserve IA slots for the agents/text-to-SQL product** | Add a nav placeholder + overview stub for the agents platform and NL querying, so the IA reflects the real product and crawlers/LLMs find it. Flesh out over time. | M | High |
| B3 | **Add a small "Operations & access" group** | Centralize scattered cross-cutting facts: auth model, egress IP `35.244.14.238` for allowlisting, base URLs (`api.actioneer.com`), least-privilege/governance posture, a short glossary. | S | Med |

*Brand fit: 2–4 tabs **max**, not Dodo's 8. The Operations/governance home reinforces the governance-first pillar and gives security reviewers a predictable destination. **Do NOT** replicate Dodo's sprawling Features + Developer-Resources + Integrations breadth.*

### C. Content depth & structure

| # | Recommendation | What to do | Effort | Impact |
|---|---|---|---|---|
| C1 | **Add "Next steps" `<CardGroup>` to every how-to page** | Append a terse 2-col card block to each warehouse/MMP/ingestion subpage routing to the logical next doc (e.g. ingestion: identity → properties → reliability chain). Keep the existing support card below. | S | High |
| C2 | **Create a thin Concepts layer (3–4 pages)** | New "Concepts" group: "How Actioneer reads your data" (context layer, read-only, governance), "Identity resolution model" (`distinct_id`, `$identify`, anon→known merge), "Ingestion vs warehouse connections — which do I use." Prose + one diagram + "Next steps" card each. | M | High |
| C3 | **Add an Errors & troubleshooting reference** | `ingestion/errors.mdx` under the API Reference group: status-code table (400/401/409/413/429/5xx) with a **retryable?** column, then a catalog of named validation/auth/dedup failures with **Code / Cause / Resolution** — crucially the Resolution column Dodo omits. Cross-link from every endpoint + reliability page. | M | High |
| C4 | **Add one centralized FAQ** | `faq.mdx` grouped by `<AccordionGroup>` (Security & governance, Connections, Ingestion, Onboarding). Move duplicated cross-cutting Q&As (e.g. the SOC2/AES-256 answer) here; per-page accordions keep only provider-specific items + a link. | M | Med |
| C5 | **Add 2–3 task-shaped recipe guides** | A "Guides" group: e.g. "Instrument an app and send your first events," "Connect a warehouse and validate read-only access." `<Steps>`, reused snippets, "Next steps" cards. | L | Med |

*Brand fit: the concept layer is where the owned/auditable/read-only/context-layer vocabulary belongs. The Resolution column is a genuine differentiator vs Dodo. **Do NOT** copy Dodo's "Billing Deconstructions of named competitors" — keep recipes generic and capability-focused.*

### D. API reference & developer experience

| # | Recommendation | What to do | Effort | Impact |
|---|---|---|---|---|
| D1 | **Add cURL + language code tabs** | Wrap the `X-API-Key` auth example and `/capture`, `/batch` envelopes in `<CodeGroup>` with cURL / Node (fetch) / Python (requests), each a complete POST with the header and 5xx backoff. Optionally add `x-codeSamples` to the OpenAPI spec so endpoint pages inherit them. | M | High |
| D2 | **Verify & harden the "Try it" playground** | Inspect `api-reference/openapi.json` for `servers` (`https://api.actioneer.com`) + the `X-API-Key` `apiKey/header` security scheme + examples. If absent, fix the spec or change the `api-reference.mdx` "in-browser request builder" line to match reality. Confirm with `mint dev`. | S | Med |
| D3 | **Add base-URLs/environments + rate-limit note to the API intro** | Expand the one-line "production and staging are distinct hosts" into an Environment \| Base URL table, plus a short Rate limits subsection (only what actually exists; label staging "provided at onboarding" rather than fabricating). | S | Med |
| D4 | **Surface `event_id` idempotency on endpoint pages** | An `<Info>` beside the envelope table (and on capture/batch) restating `event_id` is the dedup key, linking to `reliability`. | S | Med |
| D5 | **Add one canonical server-side reference snippet** | On `sending-events.mdx`, a Node + Python `<Tabs>` snippet posting an envelope with `X-API-Key`, a stable `event_id`, and 5xx backoff+jitter — operationalizing the existing checklist. | S | Med |

*Brand fit: deterministic, auditable error handling is governance-first. **Do NOT** copy Dodo's 10-language SDK matrix or build SDK pages — we have no first-party libraries; 2–3 languages and one honest reference snippet match our minimalist identity.*

### E. Visual design & polish

| # | Recommendation | What to do | Effort | Impact |
|---|---|---|---|---|
| E1 | **Convert hero images to `<Frame>`** | Replace bare `<img>` heroes in `ingestion.mdx`, `data-warehouses.mdx`, `mmp.mdx` with captioned `<Frame>`; make it the standard for all future images. | S | Med |
| E2 | **Use existing provider SVG logos as card icons** | On `data-warehouses.mdx`, swap generic Lucide icons for the brand SVGs already in `images/` (`<Card icon="/images/snowflake.svg">`); add the few missing logos. | S | Med |
| E3 | **Add monochrome data-flow / architecture diagrams** | Two `<Frame>`'d diagrams: app → JSON-over-HTTPS → context layer (ingestion); Actioneer querying sources in place, read-only egress from `35.244.14.238` (warehouses). Carbon Black / Porcelain / Mineral Green only. | M | High |
| E4 | **Add UI screenshots to in-product connection steps** | In each provider's "Connect in Actioneer" step, `<Frame>`'d screenshots of the Data sidebar / source picker / connection form (reuse a shared set where UI is identical). Dark mode to match default. | M | High |
| E5 | **Introduce `<Tabs>` and `<Check>` sparingly** | `<Tabs>` for multi-path content (SDK snippets, auth variants); one `<Check>` at the end of each connection flow. | S | Med |

*Brand fit: `<Frame>` and a restrained monochrome logo grid read enterprise and curated. **Do NOT** over-tab (Dodo's 4-way persona tabs) or add marketing art — restrained technical diagrams only.*

### F. Trust, retention & extras

| # | Recommendation | What to do | Effort | Impact |
|---|---|---|---|---|
| F1 | **Add a Changelog** | `changelog.mdx` as a top-level group/tab using native `<Update label="2026-06" …>`, newest first. Scope to docs-relevant changes (new connectors, ingestion API additions, auth changes). Enable RSS if available. | M | High |
| F2 | **Add a Support & Security page** | `support.mdx`: `<CardGroup>` per channel (Cmd+K + Ask AI = instant; `support@actioneer.com`; Dashboard → Get Support with a response-time expectation) + a "Report a security issue" block (`security@`, responsible disclosure). | S | High |
| F3 | **Turn on the feedback loop** | Add to `docs.json`: `feedback: { thumbsRating, suggestEdit, raiseIssue }`, `suggestEdit` pointed at the repo. Zero per-page authoring. | S | Med |
| F4 | **Fix & broaden the conversion path** | Repoint the navbar primary off `sentinel.gameramp.com` to a branded host; add a low-key "Talk to an engineer" footer/navbar link. One primary action per page. | S | Med |
| F5 | **Configure analytics** | Add an `analytics` block (prefer Plausible/PostHog for a privacy-respecting, data-ownership posture). Use search-query data to prioritize the missing intro/quickstart. | S | Med |
| F6 | **Document Ask-AI + MCP as a page** | A short "Ask Actioneer / MCP" page explaining the in-docs Ask AI and how to add the docs MCP server to an IDE/agent; link from the ingestion overview. Reference `/llms-full.txt`. | S | Low |

*Brand fit: a dated, auditable changelog and a stated security-disclosure path mirror the audit-trail pillar exactly. **Do NOT** lead with a Discord-community-first support model or a named AI mascot ("Sentra") — for Actioneer, owned channels (email/dashboard) and a plain "Ask Actioneer" are on-brand.*

---

## 4. Proposed Roadmap

### Phase 1 — Quick wins (S effort, high leverage; ship first)
- **A1** Introduction landing page + **A2** Choose-your-path block
- **C1** "Next steps" cards on every how-to page
- **F2** Support & Security page · **F3** feedback loop · **F4** fix the gameramp.com CTA
- **E1** heroes → `<Frame>` · **E2** provider SVG logos on cards
- **D2** verify/fix the "Try it" playground promise · **D3** base-URLs + rate-limit note · **D4** `event_id` idempotency on endpoint pages
- **B3** Operations & access group

### Phase 2 — Foundations (M effort; structure to grow into)
- **B1** top-nav tabs · **B2** reserve agents/text-to-SQL IA slots (stubs)
- **A3** canonical Quickstart · **A4** verify-first milestone · **A5** agent/MCP onboarding
- **C2** Concepts layer · **C3** Errors & troubleshooting reference · **C4** central FAQ
- **D1** code-language tabs · **D5** reference server-side snippet
- **F1** Changelog · **F5** analytics · **F6** Ask-AI/MCP page

### Phase 3 — Differentiators (M/L; what sets us apart)
- **E3** monochrome architecture/data-flow diagrams · **E4** in-product UI screenshots · **E5** `<Tabs>`/`<Check>`
- **C5** task-shaped recipe guides
- **Flesh out the Platform/Agents tab** (B2) into real concept + how-to + reference docs for the benchmark-leading text-to-SQL / agents product — the largest missing surface and our strongest proof point.

---

## 5. Open Questions for the User

1. **Scope: do we document the agents / text-to-SQL product now, or only reserve the IA slot?** This is the biggest decision — it's our differentiator (DABstep/KRAMAbench) and currently invisible in docs. Stub-now-flesh-later, or commit to full docs in this cycle?
2. **Tabs vs flat sidebar:** are you comfortable restructuring `docs.json` into top-nav tabs (B1)? This is a visible navigation change for existing users.
3. **Changelog:** do we want one, and is there a source of truth (release notes, PR history) to seed it — or do we start it forward-only from today?
4. **The `sentinel.gameramp.com` Dashboard link** — what's the correct branded URL, and do you want a second "Talk to an engineer / Contact sales" CTA, or keep docs to a single action?
5. **Support & security contacts:** can we publish `support@actioneer.com` / `security@actioneer.com` (or the real addresses) and a stated response-time expectation? F2 needs confirmed channels and SLA.
6. **Screenshots & analytics:** can you supply (or grant access to capture) dark-mode product screenshots for E4, and which analytics provider is acceptable under your data-ownership posture (Plausible / PostHog / GA)?
