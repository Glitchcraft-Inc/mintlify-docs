# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A [Mintlify](https://mintlify.com) documentation site for **Actioneer**, an enterprise AI transformation platform that helps you deploy AI agents in production. Actioneer stack includes a full-stack CDP, agents platform, with full natural language querying. Content is authored in `.mdx` files; there is no application code, build step, or test suite — the site is rendered by Mintlify from `docs.json` plus the MDX pages.

## Brand voice (write docs to match)

Actioneer's brand is **enterprise-grade, direct, and hype-free** — explain technical concepts plainly and lead with concrete use cases, not abstract promises. The core pillars, which the integration docs already echo, are:

- **Governance-first** — data teams own definitions; everything is **auditable** (audit trails, observability).
- **Operational continuity** — AI fits *into* existing systems rather than replacing them.
- **Technical Expertise** - Currently leading public text-to-SQL benchmarks like DABstep, DA Bench, KRAMAbench

Recurring vocabulary: *capabilities, workflow, owned, governance, read-only, audit trail, context layer.* Visual identity is minimalist (monochrome logo, generous white space). Typography is **Geist** end to end — Geist Sans for headings and body, **Geist Mono** for code — loaded via an `@import` in `style.css` plus `font.family: "Geist"` in `docs.json`. Do not reintroduce the older Newsreader/Inter pairing.

## Commands

```bash
mint dev          # Local preview server at http://localhost:3000 (install once: npm i -g mint)
mint broken-links # Validate all internal links resolve
mint upgrade      # Update the mint CLI
```

There is no lint/test/build command. Deployment is automatic via Mintlify's GitHub integration on push to `main`.

## Architecture

- **`docs.json`** is the single source of truth for site config: theme, colors, fonts, SEO, footer, navbar, and — most importantly — `navigation.tabs`, which defines the top-nav tabs and their sidebar groups. **A page does not appear in the site unless its path (without the `.mdx` extension) is listed in `docs.json`.** When adding a page, add both the file and its navigation entry.
- **Content is organized into four top-nav tabs**, mirroring the directory layout:
  - **Get Started** — `index.mdx` (the landing page, served at `/`), `quickstart.mdx`, and a `concepts/` group (`how-actioneer-reads-your-data`, `identity-resolution`, `ingestion-vs-warehouse`).
  - **Integrations** — connecting data into Actioneer. `integrations/data-warehouses/` (Snowflake, BigQuery, Redshift, Athena, S3, GCS, ClickHouse, Databricks, Postgres, Google Sheets) and `integrations/mmp/` (Mobile Measurement Partners: Adjust, AppsFlyer, Singular). Each subdirectory has an overview `.mdx` at its root plus one page per provider.
  - **Event Ingestion** — the CDP Event Ingestion API. `ingestion.mdx` is the overview; an OpenAPI-backed API Reference group (`ingestion/api-reference`, `ingestion/endpoints/capture`, `ingestion/endpoints/batch`, `ingestion/errors`) plus guides (`identity`, `properties`, `reliability`, `sending-events`).
  - **Resources** — `support.mdx`, `faq.mdx`, `changelog.mdx`, `resources/ask-actioneer.mdx`, and an `operations/` group (`access-and-networking`, `glossary`).
- **`style.css`** holds custom CSS overrides (the "Mineral Trust" color system, the Geist/Geist Mono `@import`, card/table styling); `logo/`, `images/`, and `favicon.svg` are static assets referenced from `docs.json` and pages.
- **`.mintignore`** excludes draft content (`drafts/`, `*.draft.mdx`) from the build.

## Page conventions (follow existing pages)

- **Frontmatter**: every page starts with YAML frontmatter. Standard keys used across the repo: `title`, `sidebarTitle`, `description`, `og:title`, `og:description`, and `keywords` (array). Match the depth of frontmatter on neighboring pages — integration pages carry full SEO/og/keywords blocks.
- **Page width**: **do not set `mode: "wide"`.** Every page uses Mintlify's default centered/readable column with a right-rail TOC; keep it that way for consistency across the site.
- **Do not repeat the title as an `# H1`** — Mintlify renders the frontmatter `title` as the page heading. (A recent commit removed duplicate H1s; don't reintroduce them.)
- Provider/integration pages open with a two-column "At a glance" key-value summary table (Prerequisites, access granted, estimated time, Actioneer IP, etc.) followed by a `<Steps>` walkthrough, using Mintlify components like `<Info>`, `<Steps>`, `<Step>`, and ` ```sql expandable ` fenced blocks.
- Leaf pages close with a `## Next steps` `<CardGroup>` routing to the logical next page(s), and provider pages keep a final `<Card title="Need a hand?" … horizontal>` support card. Cross-cutting facts (encryption/SOC 2, egress IP, base URLs) live canonically in `operations/access-and-networking.mdx` and `faq.mdx` — link to them rather than duplicating.
- The fixed Actioneer egress IP referenced in connection setup is `35.244.14.238`; the ingestion API base URL is `https://api.actioneer.com`.
