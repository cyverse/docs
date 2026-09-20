# Agent guide: CyVerse Core Software Documentation

This repository is the technical documentation for deploying, operating, and
integrating with CyVerse, built with [Zensical](https://zensical.org) and
published to https://docs.cyverse.org. The `docs/` tree is an **Open Knowledge
Format (OKF) v0.2 knowledge bundle**
([spec](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)):
every content page carries YAML frontmatter with `type`, `title`,
`description`, `tags`, provenance (`generated`, `sources`), and lifecycle
(`status`, optional `stale_after`) fields. Section `index.md` files are OKF §8
directory listings (no frontmatter; the root `index.md` declares only
`okf_version`); `docs/log.md` is the OKF §9 dated change log.

## Reading the corpus

- `docs/llms.txt`: linked outline of every page with descriptions, plus each
  page's Markdown twin (URL + `index.md`) and raw GitHub source.
- `docs/llms-full.txt`: the entire corpus in one file, frontmatter included,
  relative links rewritten to absolute URLs.
- Deployed pages carry a "View this page as Markdown" button and a
  "Machine-readable versions" line; `docs/about/ai-agents.md` is the guide.
- Trust: pages without a `verified:` key are **unverified** (OKF §5.3); every
  page produced by the migration (`generated.by: process:okf-migration`) is
  unverified. `status: deprecated` pages are history; `status: draft` pages
  need review against a current deployment.
- Hostnames, DNs, realms, and credentials in the docs are placeholders.

## Layout

The nav follows the order CyVerse is deployed: `architecture/` (what the
system is and needs), `deployment/` (`planning/`, then phases `01-foundation/`
through `07-post-install/`, ordered by dependency; `from-scratch.md` is the
end-to-end runbook), `platform/` (products), `operations/` (running a
deployment), `api/` (Terrain), `development/`, `references/` (provenance for
derived material), and `about/`. The nav is explicit in `zensical.toml`.

## Commands

```bash
pip install -r requirements.txt             # zensical, mkdocs-material (emoji), pyyaml; Python 3.10+
zensical serve                              # live preview at localhost:8000
zensical build --clean                      # static site -> site/
python3 scripts/okf_validate.py docs        # OKF conformance (CI-enforced)
python3 scripts/gen_llms_txt.py             # regenerate llms.txt indexes (CI checks drift)
python3 scripts/postbuild_agent_surface.py site  # after build: Markdown mirror (absolute
                                            #   links), okf:* meta, Markdown button +
                                            #   machine-readable line, robots.txt
```

## Editing rules

1. Every new content page needs OKF frontmatter with a non-empty `type`
   (types in use: Deployment Procedure, Playbook, Database, Service,
   Architecture Overview, API Overview, API Endpoint, Guide, Reference). Run
   `okf_validate.py` before committing; CI fails otherwise.
2. A new page must be added to the nav in `zensical.toml` **and** listed in
   its directory's `index.md`. `mkdocs.yml` is a legacy config CI does not
   use; keep its nav in step if you touch the nav.
3. Section `index.md` files never get frontmatter.
4. External links get `{target=_blank}`; internal links are relative and stay
   plain. Code belongs in fenced blocks with a language.
5. Never put real hostnames, DNs, or credentials in a page; use placeholders.
6. Meaningful changes get a dated entry in `docs/log.md` (newest first,
   `## YYYY-MM-DD` headings).
7. After content changes: `gen_llms_txt.py`, then build, then commit
   `docs/llms.txt` and `docs/llms-full.txt` with the change.
8. Never mark a page `verified:`; only CyVerse staff do that, as
   `verified: { by: "human:<username>", at: <ISO 8601> }`.
9. `scripts/okf_common.py` holds the shared helpers (config loading, nav
   walking, link absolutizing), adapted from UNM-CARC/docs; all scripts read
   site settings from `zensical.toml`.
10. Pushes to `main` deploy publicly to docs.cyverse.org; work on a branch
    and open a pull request (CI validates and trial-builds PRs).
