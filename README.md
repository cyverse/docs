A special thanks to [Mojib Wali](https://github.com/mb-wali) of [CyVerse-Austria](https://www.tugraz.at/sites/cyverse/home), Graz University of Technology, Austria, for creating the initial Material MkDocs pages for the global CyVerse developer and maintainer community!

CyVerse Core Software Documentation
===================================

These documents are for the deployment and maintenance of [CyVerse](https://cyverse.org)

## Zensical Static Site Generator

This documentation is built using [Zensical](https://zensical.com), a modern static site generator from the team behind Material for MkDocs. The site uses Zensical's "classic" theme which provides the familiar Material for MkDocs appearance with enhanced features.

## GitHub Actions Deployment

The `main` branch uses GitHub Actions to automatically build and deploy the documentation.

Do not commit changes directly to the `main` branch unless necessary. Please commit your updates to the `mkdocs` branch, test on CodeSpaces or locally, and then commit those merges back to `main`.

Commits to `main` will trigger the Action which re-builds and deploys the website to https://docs.cyverse.org -- which is publicly available.

## Configuration

The documentation is configured using `zensical.toml` (TOML format) instead of the traditional `mkdocs.yml`. This provides better configuration management and access to Zensical-specific features.

## Agent-ready documentation (OKF v0.2)

`docs/` is an [Open Knowledge Format v0.2](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) knowledge bundle: every page carries YAML frontmatter (type, provenance, lifecycle). The published site exposes it to AI agents through [`llms.txt`](https://docs.cyverse.org/llms.txt), [`llms-full.txt`](https://docs.cyverse.org/llms-full.txt), and a Markdown copy of every page at its URL + `index.md`. See [For AI agents](https://docs.cyverse.org/about/ai-agents/) and, for coding agents working in this repository, [AGENTS.md](AGENTS.md).

Before committing content changes:

```bash
python3 scripts/okf_validate.py docs   # OKF conformance (CI-enforced)
python3 scripts/gen_llms_txt.py        # regenerate docs/llms.txt and docs/llms-full.txt (CI checks drift)
```

## Local Development

```bash
git clone https://github.com/cyverse/docs
cd docs
pip install -r requirements.txt
zensical serve
```

After starting the development server, the site will open at `http://localhost:8000`

Open in browser and view

Recommended: two monitors for easier display while working on docs.

## Building

To build the static site:

```bash
zensical build
```

Output will be in the `./site` directory. 