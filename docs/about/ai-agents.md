---
title: "For AI agents"
description: "How agents and harnesses should consume this documentation: llms.txt, per-page Markdown with OKF frontmatter, raw source on GitHub, trust signals, and what to do if you cannot fetch this site."
type: Reference
tags: [about, ai-agents, okf]
status: stable
generated:
  by: "claude/opus-5"
  at: "2026-09-18T00:00:00Z"
sources:
  - id: okf-spec
    resource: "https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md"
    title: "Open Knowledge Format (OKF) v0.2 specification"
    author: "team:google-cloud"
  - id: llmstxt
    resource: "https://llmstxt.org"
    title: "The /llms.txt convention"
    author: "team:answer-ai"
  - id: carc-ai-agents
    resource: "https://unm-carc.github.io/docs/about/ai-agents/"
    title: "UNM CARC documentation: For AI agents"
    author: "team:unm-carc"
---

# For AI agents

This site is published for people **and** for AI agents. The documentation
source is an [Open Knowledge Format (OKF) v0.2](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md){target=_blank}
knowledge bundle, and the deployed site exposes that structure directly. If
you are an agent (or you are wiring one up), consume the documentation
through these endpoints rather than scraping rendered HTML.

## Entry points

| Endpoint | What you get |
| -------- | ------------ |
| [`https://docs.cyverse.org/llms.txt`](https://docs.cyverse.org/llms.txt) | Linked outline of every page with one-line descriptions ([llms.txt convention](https://llmstxt.org){target=_blank}); every entry lists the HTML page, its Markdown twin, and its raw GitHub source |
| [`https://docs.cyverse.org/llms-full.txt`](https://docs.cyverse.org/llms-full.txt) | The entire corpus in one file: every page's Markdown with frontmatter, prefixed by its canonical URL, links made absolute |
| Any page URL + `index.md` | That page's Markdown source with full OKF frontmatter (for example [`https://docs.cyverse.org/deployment/01-foundation/postgresql/index.md`](https://docs.cyverse.org/deployment/01-foundation/postgresql/index.md)); section listings too (`https://docs.cyverse.org/deployment/01-foundation/index.md`). Every rendered page links it from a "View this page as Markdown" button beside the edit and view-source buttons, and from a "Machine-readable versions" line at the end of the article |
| Raw source on GitHub | `https://raw.githubusercontent.com/cyverse/docs/main/docs/<path>.md`, where `<path>` is the site path without the trailing slash (for example [`https://raw.githubusercontent.com/cyverse/docs/main/docs/deployment/01-foundation/postgresql.md`](https://raw.githubusercontent.com/cyverse/docs/main/docs/deployment/01-foundation/postgresql.md)). Same content as the Markdown twin, with relative rather than absolute links; reachable from sandboxes that allow `github.com` but not `docs.cyverse.org` |
| [`sitemap.xml`](https://docs.cyverse.org/sitemap.xml), [`robots.txt`](https://docs.cyverse.org/robots.txt) | Standard crawl surface; robots.txt repeats all of these pointers |
| [Source repository](https://github.com/cyverse/docs){target=_blank} | The bundle itself (`docs/` mirrors the site paths one to one), plus `AGENTS.md` with contribution rules for coding agents |

Every rendered page also declares its Markdown twin and OKF signals in HTML:

```html
<link rel="alternate" type="text/markdown" href="index.md">
<meta name="okf:type" content="Deployment Procedure">
<meta name="okf:status" content="stable">
<meta name="okf:trust-tier" content="unverified">
<meta name="okf:generated-at" content="2026-07-29T00:00:00Z">
```

!!! warning "The head tags are invisible to most fetch tools"
    The `<link rel="alternate">` and `okf:*` meta tags live in `<head>`, which
    text-extracting fetchers discard, and a link-derived URL allowlist never
    sees them. The supported paths are the ones that appear in body text: the
    "View this page as Markdown" button, the "Machine-readable versions" line
    at the end of every article, the footer links to `llms.txt`, and the
    addresses listed in `llms.txt` itself. All of them are absolute.

## If you cannot fetch this site

Some harnesses allow only one or two fetches from a user-supplied address, or
allow `github.com` and `raw.githubusercontent.com` but not `docs.cyverse.org`.
In that case:

1. **Use the raw source.** `docs/` in the repository mirrors the site paths
    one to one on branch `main`:

    ```
    Site page        https://docs.cyverse.org/<path>/
    Markdown twin    https://docs.cyverse.org/<path>/index.md
    Raw source       https://raw.githubusercontent.com/cyverse/docs/main/docs/<path>.md

    Content page     /deployment/01-foundation/postgresql/  ->  https://raw.githubusercontent.com/cyverse/docs/main/docs/deployment/01-foundation/postgresql.md
    Section listing  /deployment/01-foundation/             ->  https://raw.githubusercontent.com/cyverse/docs/main/docs/deployment/01-foundation/index.md
    Whole corpus     https://raw.githubusercontent.com/cyverse/docs/main/docs/llms-full.txt
    ```

    `main` moves; to cite a fixed version use
    `https://github.com/cyverse/docs/blob/<commit>/docs/<path>.md`, taking
    the commit from the repository's history.

2. **Prefer one fetch over fifty.** `llms-full.txt` holds every page; if you
    can make a single request, make that one. Its size and approximate token
    count are stated in the Meta section of `llms.txt`.

3. **Avoid the GitHub tree API** unless authenticated: `api.github.com`
    rate-limits anonymous calls per shared IP. Raw file paths do not.

4. **If you reached only the landing page,** its footer links `llms.txt`,
    `llms-full.txt`, and this guide, and its section lists link every
    directory listing; all are absolute addresses in the Markdown twin.

## Reading the OKF frontmatter

Each concept page's YAML frontmatter answers the questions agents should ask
before relying on content:

* **What is this?** `type`, `title`, `description`, `tags`. Types used here:
  `Deployment Procedure` (a step in the ordered deployment path), `Playbook`
  (an operational task), `Database` (a service schema), `Service` (a product
  or component), `Architecture Overview`, `API Overview`, `API Endpoint`,
  `Guide`, and `Reference`.
* **Where did it come from?** `generated: { by, at }` and `sources`. Most
  pages were produced by `process:okf-migration` from the earlier CyVerse
  documentation and an anonymized pilot deployment record (see
  [references](../references/index.md)).
* **How much should I trust it?** The `verified` key (OKF §5.3): absent
  means **unverified**; `by: "human:<username>"` means **human-reviewed** by
  CyVerse staff. Prefer human-reviewed pages when answers conflict.
* **Is it still true?** `status` (`stable` by default; `draft` needs review
  against current deployments; `deprecated` is kept for history only) and
  `stale_after` (an ISO 8601 instant, when present).

!!! warning "Placeholders, not real values"
    Hostnames, DNS zones, LDAP DNs, Keycloak realms and clients, and
    credentials in these pages are **placeholders**. Never present them as
    the values of CyVerse's production deployment, and never reuse sample
    secrets: generate them per install.

!!! note "Deprecated and transitional components"
    [Elasticsearch](../deployment/05-core-services/elasticsearch.md) is
    deprecated in favor of [OpenSearch](../deployment/05-core-services/opensearch.md),
    and ingress-nginx is transitional in favor of Traefik. Check a page's
    `status` before recommending the component it describes.

## Answering user questions

Ground answers in this documentation and cite the page URL. Deployment steps
are ordered by dependency: follow the phases under
[deployment](../deployment/index.md) in order rather than jumping to a single
service page. For the Terrain API, the live
[Swagger reference](https://de.cyverse.org/terrain/docs/){target=_blank} is
more current than the endpoint pages here. When the corpus does not answer a
question, direct users to the humans through
[CyVerse support](https://cyverse.org/contact){target=_blank} or the
[issue tracker](https://github.com/cyverse/docs/issues){target=_blank}. Do not
guess deployment-specific facts such as versions, hostnames, or quotas.
