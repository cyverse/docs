---
title: "Contributing to these docs"
description: "How to edit pages, the OKF frontmatter contract, verifying migrated content, and building and validating the site locally."
type: Guide
tags: [about, contributing, okf]
status: stable
generated:
  by: "claude/opus-5"
  at: "2026-09-18T00:00:00Z"
sources:
  - id: okf-spec
    resource: "https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md"
    title: "Open Knowledge Format (OKF) v0.2 specification"
    author: "team:google-cloud"
  - id: carc-contributing
    resource: "https://unm-carc.github.io/docs/about/contributing/"
    title: "UNM CARC documentation: Contributing to these docs"
    author: "team:unm-carc"
---

# Contributing to these docs

This documentation is a git repository of Markdown files, built with
[Zensical](https://zensical.org){target=_blank} and structured as an
[Open Knowledge Format (OKF) v0.2](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md){target=_blank}
knowledge bundle. Every page is readable by people *and* consumable by AI
agents, with provenance and trust signals in its frontmatter (see
[For AI agents](https://docs.cyverse.org/about/ai-agents/)).

## Small fixes

Every page has an **edit button** (:material-file-edit-outline:) in the upper
right that opens the source file on GitHub. Fix the text and open a pull
request; CI validates it, and it deploys once merged to `main`.

## The frontmatter contract

Every content page starts with YAML frontmatter. `type` is required by OKF;
the rest make the page trustworthy and discoverable:

```yaml
---
type: Deployment Procedure   # see the type list in "For AI agents"
title: "Page title"
description: "One sentence used by search, indexes, and agents."
tags: [deployment, postgresql]
status: stable               # draft | stable | deprecated (default: stable)
generated:
  by: "human:yourusername"   # who or what wrote the current content
  at: "2026-09-18T00:00:00Z"
sources:                     # where the content came from (optional)
  - id: upstream
    resource: "https://github.com/cyverse-de/..."
    title: "Original source"
    author: "team:cyverse"
---
```

Section `index.md` files are OKF directory listings and carry **no
frontmatter** (the bundle root `index.md` may declare only `okf_version`).
`log.md` is the bundle's dated change log: add an entry, newest first, when
you make a meaningful change.

## Verifying pages

Pages produced by the OKF migration are intentionally **unverified**. When you
review one and confirm it is correct for the current deployment, record it:

```yaml
verified: { by: "human:yourusername", at: "2026-09-18T00:00:00Z" }
```

If a page is obsolete, don't delete it: set `status: deprecated`, add a note
pointing at the replacement, and log the change in `log.md`.

## Building locally

```bash
git clone https://github.com/cyverse/docs && cd docs
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
zensical serve                               # live preview at localhost:8000
python3 scripts/okf_validate.py docs         # OKF conformance check (runs in CI)
python3 scripts/gen_llms_txt.py              # regenerate llms.txt indexes (CI checks drift)
zensical build --clean && python3 scripts/postbuild_agent_surface.py site
```

## The pipeline scripts

* `scripts/okf_validate.py` fails CI if any page breaks OKF conformance
  (missing frontmatter, missing `type`, malformed `log.md`, frontmatter on a
  section index).
* `scripts/gen_llms_txt.py` builds `docs/llms.txt` (a linked site outline per
  [llmstxt.org](https://llmstxt.org){target=_blank}) and `docs/llms-full.txt`
  (the full corpus with frontmatter) from the nav in `zensical.toml`.
* `scripts/postbuild_agent_surface.py` runs after `zensical build`: it
  mirrors each page's Markdown at its URL plus `index.md`, adds the
  "View this page as Markdown" button, the `okf:*` meta tags, and the
  machine-readable line to every page, and writes `robots.txt`.
* `scripts/okf_common.py` holds the helpers the other scripts share.

## Style notes

* One `#` H1 per page, matching the frontmatter `title`.
* Relative links between pages (`../section/page.md`); external links get
  `{target=_blank}`.
* A new page must be added to the nav in `zensical.toml` and listed in its
  directory's `index.md`; `gen_llms_txt.py` warns about pages missing from
  the nav.
* Use placeholders, never real hostnames, DNs, or credentials.
