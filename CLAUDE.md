# Marketing- : Bundled Claude Code Marketing Toolkit

## Project Overview

This repository (`Evenmore2018/Marketing-`) is a **marketing toolkit for Claude Code**
that bundles two independent Claude Code plugins in one checkout:

| Plugin | Location | Version | Author | What it does |
|---|---|---|---|---|
| **claude-seo** | repo root (`skills/`, `agents/`, `scripts/`, …) | v2.2.0 | AgriciDaniel | Tier‑4 SEO analysis skill: 25 sub‑skills, 18 sub‑agents, 50 Python scripts covering technical SEO, content/E‑E‑A‑T, schema, sitemaps, Core Web Vitals, local SEO, backlinks, AI/GEO, e‑commerce, hreflang, SXO, clustering, drift, and Google APIs. |
| **marketing-skills** | `marketingskills/` | v2.8.10 | Corey Haines | 47 model‑invoked marketing skills: CRO, copywriting, cold email, prospecting, paid ads, ad creative, SMS, pricing, referrals, RevOps, PR, launches, and more. |

The root `.claude-plugin/plugin.json` / `marketplace.json` describe **claude-seo** —
it is the primary plugin and the source of the bulk of this repo. The
`marketingskills/` subtree is the unpacked upstream
[`coreyhaines31/marketingskills`](https://github.com/coreyhaines31/marketingskills)
plugin, kept self‑contained with its own manifest, README, and `skills/`.

> **Note on provenance:** claude-seo upstream lives at
> [`AgriciDaniel/claude-seo`](https://github.com/AgriciDaniel/claude-seo) (public) and
> a private mirror. Those repos have their own release ceremony (public/private
> promotion, `/release-blog`, tagging). **None of that applies here** — this repo is a
> downstream bundle, not the claude-seo dev repo. Work in this repo follows the git
> workflow in [Git Workflow](#git-workflow) below.

---

## Plugin 1 — claude-seo (repo root)

Follows the Agent Skills open standard and a 3‑layer architecture (directive,
orchestration, execution). Progressive disclosure: metadata always loaded,
instructions on activation, resources on demand.

### Architecture

```
/ (claude-seo plugin root)
  CLAUDE.md                          # This file
  .claude-plugin/
    plugin.json                    # claude-seo manifest (v2.2.0)
    marketplace.json               # Marketplace catalog
  skills/                            # 25 sub-skills (auto-discovered)
    seo/                           # Main orchestrator (SKILL.md + references/)
    seo-audit/  seo-page/  seo-technical/  seo-content/  seo-schema/
    seo-sitemap/  seo-images/  seo-geo/  seo-local/  seo-maps/  seo-plan/
    seo-programmatic/  seo-competitor-pages/  seo-hreflang/  seo-google/
    seo-backlinks/  seo-cluster/  seo-sxo/  seo-drift/  seo-ecommerce/
    seo-dataforseo/  seo-image-gen/            # extension mirrors
  agents/                            # 18 subagents (auto-discovered, *.md)
  hooks/
    hooks.json                     # PostToolUse Edit|Write -> validate-schema.py
    run-python-hook.js             # Node shim that runs the Python hook
  scripts/                           # 50 tracked Python scripts (+ dev-only helpers)
  schema/                            # Schema.org JSON-LD templates
  extensions/                        # Optional MCP add-on installers
    dataforseo/  firecrawl/  banana/  (+ ahrefs, se-ranking, profound, bing, unlighthouse)
  tests/                             # 26 pytest modules (README badge: 326 passing)
  docs/                              # Extended documentation
  requirements.txt  pyproject.toml   # Python deps + packaging
  install.sh / install.ps1           # Installers (uninstall.sh / .ps1)
  .devcontainer/devcontainer.json    # Python 3.12 dev container
```

### Skills (auto-discovered under `skills/`)

`seo` (orchestrator), `seo-audit`, `seo-page`, `seo-technical`, `seo-content`,
`seo-schema`, `seo-sitemap`, `seo-images`, `seo-geo`, `seo-local`, `seo-maps`,
`seo-plan`, `seo-programmatic`, `seo-competitor-pages`, `seo-hreflang`,
`seo-google` (+10 API reference files), `seo-backlinks`, `seo-cluster`,
`seo-sxo`, `seo-drift`, `seo-ecommerce`, `seo-dataforseo`, `seo-image-gen`.

### Agents (`agents/*.md`, invoked via the Agent tool — never via Bash)

`seo-technical`, `seo-content`, `seo-schema`, `seo-sitemap`, `seo-performance`,
`seo-visual`, `seo-geo`, `seo-local`, `seo-maps`, `seo-google`, `seo-backlinks`,
`seo-dataforseo`, `seo-image-gen`, `seo-cluster`, `seo-sxo`, `seo-drift`,
`seo-ecommerce`.

### Key scripts (`scripts/`, 50 tracked)

Auth/safety: `google_auth.py`, `backlinks_auth.py`, `url_safety.py`.
Google APIs: `pagespeed_check.py`, `crux_history.py`, `lcp_subparts.py`,
`gsc_query.py`, `gsc_inspect.py`, `indexing_notify.py`, `ga4_report.py`,
`nlp_analyze.py`, `youtube_search.py`, `keyword_planner.py`.
Backlinks: `moz_api.py`, `bing_webmaster.py`, `commoncrawl_graph.py`,
`verify_backlinks.py`, `validate_backlink_report.py`.
Fetch/render: `fetch_page.py`, `parse_html.py`, `render_page.py`,
`capture_screenshot.py`, `analyze_visual.py`, `preload_check.py`,
`agent_ux_check.py`.
Content: `content_quality.py`, `content_humanize.py`, `content_verify.py`.
Schema: `schema_generate.py`, `schema_ecommerce_validate.py`, `iptc_ai_label.py`.
Drift: `drift_baseline.py`, `drift_compare.py`, `drift_report.py`,
`drift_history.py`.
DataForSEO: `dataforseo_costs.py`, `dataforseo_merchant.py`, `dataforseo_normalize.py`.
Reporting: `google_report.py` (canonical report generator).
Ops/misc: `sync_flow.py`, `parasite_risk.py`, `gbp_deprecation_lint.py`,
`domain_history.py`, `seo_updates.py`, `indexnow_submit.py`, `ucp_check.py`,
`unlighthouse_run.py`, `portability_check.py`, `release_sign.py`,
`verify_release.py`.

### Commands (from the `seo` orchestrator)

| Command | Purpose |
|---|---|
| `/seo audit <url>` | Full site audit with up to 15 parallel subagents |
| `/seo page <url>` | Deep single-page analysis |
| `/seo technical <url>` | Technical SEO audit (9 categories) |
| `/seo content <url>` | E-E-A-T and content quality analysis |
| `/seo content-brief <topic>` | SEO content brief: keywords, outline, internal links |
| `/seo schema <url>` | Schema.org detection, validation, generation |
| `/seo sitemap <url>` | XML sitemap analysis or generation |
| `/seo images <url or optimize>` | Image SEO: on-page audit, SERP analysis, optimization |
| `/seo geo <url>` | AI search / Generative Engine Optimization |
| `/seo plan <type>` | Strategic SEO planning by industry |
| `/seo programmatic` | Programmatic SEO analysis and planning |
| `/seo competitor-pages` | Competitor comparison page generation |
| `/seo local <url>` | Local SEO (GBP, citations, reviews, map pack) |
| `/seo maps [command] [args]` | Maps intelligence (geo-grid, GBP audit, reviews, competitors) |
| `/seo hreflang <url>` | International SEO / hreflang audit |
| `/seo google [command] [url]` | Google SEO APIs (GSC, PageSpeed, CrUX, Indexing, GA4) |
| `/seo backlinks <url>` | Backlink profile analysis (Moz, Bing, Common Crawl; premium: DataForSEO) |
| `/seo backlinks setup` \| `verify <url>` | Setup free backlink APIs; verify known backlinks |
| `/seo cluster <seed-keyword>` | SERP-based semantic clustering |
| `/seo sxo <url>` | Search Experience Optimization |
| `/seo drift baseline\|compare\|history <url>` | SEO drift monitoring |
| `/seo ecommerce <url>` | E-commerce SEO: product schema, marketplace intelligence |
| `/seo flow <url>` | FLOW framework: stage prompts + structured output |
| `/seo firecrawl \| dataforseo \| image-gen …` | Extension commands |

### claude-seo development rules

- Keep SKILL.md files under 500 lines / 5000 tokens; reference files under 200 lines.
- Scripts must have docstrings, a CLI interface, and JSON output.
- kebab-case naming for all skill directories.
- Agents are invoked via the Agent tool, **never** via Bash.
- Python deps install into `~/.claude/skills/seo/.venv/` (repo dev container uses Python 3.12).
- Run `python3 -m pytest tests/` after changes.

### claude-seo security rules

- **Never commit credentials.** `.env`, `client_secret*.json`, `oauth-token.json`,
  `service_account*.json` are all in `.gitignore`.
- **URL validation.** Scripts that accept user URLs must call `validate_url()` from
  `google_auth.py` (or use `url_safety.py`) before making requests — blocks private
  IPs, loopback, and GCP metadata endpoints (SSRF / DNS-rebinding protection).
- **OAuth tokens.** Never store `client_secret` in the token file; read it from
  `client_secret.json` at runtime.
- **No hardcoded paths.** Use `os.path.dirname(os.path.abspath(__file__))`.
- **Config location.** `~/.config/claude-seo/google-api.json` and
  `~/.config/claude-seo/backlinks-api.json` (user-space, not in repo).

### Report generation rules

- **All SEO reports use `scripts/google_report.py`** (canonical generator).
- Deps: `matplotlib>=3.8.0` + `weasyprint>=61.0` (both in `requirements.txt`).
- Format: A4 PDF via WeasyPrint + matplotlib charts at 200 DPI.
- Palette: navy `#1e3a5f`, dark gold `#b8860b`, forest green `#2d6a4f` (pass),
  warm amber `#d4740e` (warn), deep red `#c53030` (fail), cream `#faf9f7` (bg).
  Times New Roman body font.
- Structure: Title → TOC w/ scores → Executive Summary → Data → Recommendations → Methodology.
- Charts 85% width, max-height 120mm, captioned, saved to `charts/` at 200 DPI.
- **No `page-break-inside: avoid`** (causes WeasyPrint white gaps).
- `_review_pdf()` runs automatically; verify `"status": "PASS"` before presenting a PDF.
- After any analysis command, offer: "Generate a PDF report? Use `/seo google report`".

### Quality-gate hook

`hooks/hooks.json` registers a `PostToolUse` hook on `Edit|Write` that runs
`hooks/validate-schema.py` (via `hooks/run-python-hook.js`) against the edited
file — schema validation on save. `${CLAUDE_PLUGIN_ROOT}` is resolved at runtime.

---

## Plugin 2 — marketing-skills (`marketingskills/`)

Upstream: [`coreyhaines31/marketingskills`](https://github.com/coreyhaines31/marketingskills)
(MIT, v2.8.10). Self-contained: its own `.claude-plugin/plugin.json`,
`marketplace.json`, `README.md`, `VERSIONS.md`, `AGENTS.md` (symlinked as
`CLAUDE.md`), `skills/`, and `tools/`.

These are **model-invoked** skills — activated automatically from each skill's
`description` frontmatter when the user's request matches, not via `/slash`
commands.

### Structure

```
marketingskills/
  .claude-plugin/plugin.json        # marketing-skills manifest (v2.8.10)
  AGENTS.md  (CLAUDE.md -> AGENTS.md symlink)
  README.md  VERSIONS.md
  skills/                            # 47 skills, each a dir with SKILL.md (+ references/, evals/)
  tools/
    clis/                            # CLI helper docs
    composio/                        # Composio marketing-tools reference
    integrations/                    # ~40 SaaS integration guides (Stripe, Apollo,
                                     #   Mailchimp, Ahrefs, Meta/TikTok Ads, Zapier, …)
  validate-skills.sh                 # Skill validator
  validate-skills-official.sh        # Official Agent Skills spec validator
```

### The 47 skills

`product-marketing` is the **foundation** — every other skill reads it first
(checks `.agents/product-marketing.md`, `.claude/product-marketing.md`, or legacy
`product-marketing-context.md`) for product, audience, and positioning context.

Grouped roughly as:

- **SEO & Content:** `seo-audit`, `ai-seo`, `site-architecture`, `programmatic-seo`,
  `schema`, `content-strategy`, `aso`
- **CRO & Onboarding:** `cro`, `signup`, `onboarding`, `popups`, `paywalls`
- **Copy & Creative:** `copywriting`, `copy-editing`, `cold-email`, `emails`,
  `social`, `video`, `image`, `sms`, `ad-creative`
- **Paid & Measurement:** `ads`, `ab-testing`, `analytics`
- **Growth & Retention:** `referrals`, `free-tools`, `churn-prevention`,
  `community-marketing`, `lead-magnets`, `co-marketing`, `marketing-loops`
- **Sales & GTM:** `revops`, `sales-enablement`, `launch`, `pricing`,
  `competitors`, `competitor-profiling`, `directory-submissions`, `prospecting`,
  `public-relations`, `offers`
- **Strategy & Research:** `marketing-ideas`, `marketing-plan`,
  `marketing-psychology`, `customer-research`, `marketing-council`
- Plus the product foundation: `product-marketing`

Each skill's **Related Skills** section documents its cross-references.

### marketing-skills conventions

- Every skill dir contains a `SKILL.md` with `name` + `description` + `metadata.version`
  frontmatter; many add `references/` and `evals/`.
- Validate changes with `marketingskills/validate-skills.sh` (and
  `validate-skills-official.sh` for spec compliance).
- Skills follow the [Agent Skills spec](https://agentskills.io) and are portable
  across Claude Code, Codex, Cursor, and Windsurf.

---

## Git Workflow

- **Active development branch for this session:** `claude/claude-md-docs-5bpvuu`.
  Develop, commit, and push here. Create it locally from the latest default branch
  if it does not exist. Never push to another branch without explicit permission.
- **Remote:** `origin` → `Evenmore2018/Marketing-` (this is the only remote; the
  upstream public/private claude-seo topology does **not** exist in this checkout).
- **Push:** `git push -u origin <branch>`; on network errors retry up to 4× with
  exponential backoff (2s, 4s, 8s, 16s).
- Do **not** open a pull request unless explicitly asked. When you do, honor
  `.github/PULL_REQUEST_TEMPLATE.md`.
- If this branch's PR is already merged, restart the branch from the latest default
  branch rather than stacking new commits on merged history.

## Repository Meta

- **CI/tooling:** `.github/workflows/ci.yml`, `.github/workflows/v2.yml`,
  `.github/dependabot.yml`, `.github/CODEOWNERS`, issue/PR templates under
  `.github/`. `.devcontainer/devcontainer.json` provisions Python 3.12 + Playwright
  (Chromium).
- **Docs at root:** `README.md`, `CHANGELOG.md`, `AGENTS.md` (multi-platform agent
  instructions), `CONTRIBUTING.md`, `CONTRIBUTORS.md`, `SECURITY.md`, `PRIVACY.md`,
  `CODE_OF_CONDUCT.md`, `CITATION.cff`.
- **Which plugin am I editing?** Files under `marketingskills/` belong to the
  marketing-skills plugin; everything else at root belongs to claude-seo. Apply the
  matching plugin's rules above.

## Extension System

claude-seo ships optional MCP extension installers in `extensions/`: **DataForSEO**
(live SEO data), **Firecrawl** (site crawling), **Banana** (AI image generation),
plus Ahrefs, SE Ranking, Profound, Bing Webmaster, and Unlighthouse. marketing-skills
documents ~40 SaaS integrations under `marketingskills/tools/integrations/`.
