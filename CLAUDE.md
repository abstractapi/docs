# docs

**Public API documentation** for Abstract, published with [Mintlify](https://mintlify.com/) at `docs.abstractapi.com`. This repo is **content, not a service** — `.mdx` pages, navigation config, and static assets. It does **not** depend on [`commons`](../commons/CLAUDE.md) and has no Django/DRF, no metering, no request path.

> Deploys automatically: pushing to `main` publishes to production. PRs generate a Mintlify preview link. There is no build step to run in CI beyond Mintlify's own.

See [`./CONTEXT.md`](./CONTEXT.md) for this repo's vocabulary (once added).

## Stack

- **Mintlify** — docs framework. Content authored in **MDX** (`.mdx` = Markdown + JSX components).
- Local preview via the Mintlify CLI: `npm i -g mintlify`, then `mintlify dev`.
- No Python, no Django, no `commons`. Pure static docs.

## Layout

Two coexisting Mintlify configurations — the repo is **mid-migration** from the legacy per-product sites to a single unified site:

- **`api-docs/`** — the **new unified docs site** (`docs.json`, the current Mintlify schema). One site covering every product, with `navigation.dropdowns` grouping the APIs (Popular / verify / look up / create). Pages live under `api-docs/api/*.mdx`, with per-product subfolders (e.g. `api-docs/api/ip-intelligence/`, `api-docs/api/email-reputation/`) for multi-page products. This is where new work should go.
- **Per-product top-level folders** — `ip-intelligence/`, `email-validation/`, `phone-validation/`, `company-enrichment/`, `timezone/`, `holidays/`, `iban-validation/`, `vat/`, `exchange-rates/`, `avatars/`, `images/`, `scrape/`, `screenshot/`, `email-reputation/`, `phone-intelligence/`, `ip-geolocation/`. Each has its **own legacy `mint.json`** (the old single-product Mintlify schema) and its `<product>.mdx`. These are the **legacy per-product sites**.
- `images/`, `avatars/`, `screenshot/`, `scrape/` — note these are **both** product-doc folders **and** where shared static assets live; check whether a folder is a product or an asset bucket before editing.
- `logo/`, `favicon.png` — branding assets referenced from the Mintlify config.

## Entry point

```bash
npm i -g mintlify
cd api-docs        # or a legacy per-product folder — run where the config file lives
mintlify dev       # local preview at http://localhost:3000
```

- New unified site config: `api-docs/docs.json` (Mintlify `docs.json` schema).
- Legacy per-product config: `<product>/mint.json` (old `mint.json` schema).

## Content conventions

- Every page is `.mdx` with **YAML frontmatter**: `title`, `icon`, `description`, and for API reference pages an `api:` line (e.g. `api: 'GET https://ip-intelligence.abstractapi.com/v1'`).
- Product/voice/terminology must follow Abstract's brand rules — the `/copywriting` skill is the source of truth for naming, voice, and tone. Docs are customer-facing copy.
- Output language is **English (US)**, like all Abstract artifacts.

## Gotchas

- **No `commons` dependency, off the request path.** A wrong answer in the docs is a content bug, not a service bug — fix the `.mdx`, don't go looking in a service repo.
- **Two Mintlify schemas coexist.** `api-docs/docs.json` (new, unified) vs `<product>/mint.json` (legacy, per-product). Don't mix schema keys between them. Prefer the unified `api-docs/` for new content.
- **Auto-deploy on push to `main`.** Merging publishes to production immediately. Use a PR to get a preview link before shipping.
- **Docs can drift from the API.** This repo is not generated from the services; it's hand-authored. When a product changes (new field, new endpoint, renamed product — e.g. Geolocation → IP Intelligence, Email Validation → Email Intelligence), the docs must be updated here **separately**. Cross-check against the owning service repo.
- **Product naming lags reality.** The folder set still reflects legacy product names (`ip-geolocation`, `email-validation`) alongside the new ones (`ip-intelligence`, `email-reputation`). Confirm which product a page describes before editing.
- **404 in `mintlify dev`** almost always means you ran it in a folder without a config file (`mint.json` / `docs.json`). Run it from the folder that owns the config.
- **Mixpanel + social links are baked into the config.** Don't strip `integrations.mixpanel` or `footer.socials` when editing `docs.json`.

## Agent skills

### Issue tracker

Linear (via the workspace's `linear` MCP server, declared in [`abstractapi/abstract-claude-workspace`](https://github.com/abstractapi/abstract-claude-workspace)'s `.mcp.json`). Conventions live at the workspace level: see [`docs/agents/issue-tracker.md`](https://github.com/abstractapi/abstract-claude-workspace/blob/main/docs/agents/issue-tracker.md).

### Triage labels

Default mattpocock vocabulary (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`). Label creation in Linear deferred until first use. Conventions live at the workspace level: see [`docs/agents/triage-labels.md`](https://github.com/abstractapi/abstract-claude-workspace/blob/main/docs/agents/triage-labels.md).

### Domain docs

Single-context — [`CONTEXT.md`](./CONTEXT.md) at the repo root (once added). Domain conventions live at the workspace level: see [`docs/agents/domain.md`](https://github.com/abstractapi/abstract-claude-workspace/blob/main/docs/agents/domain.md).
