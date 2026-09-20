---
okf_version: "0.2"
---

![](https://docs.cyverse.org/assets/cyverse_logo_2022.svg)

*Technical documentation for deploying and operating CyVerse — the open-source
cyberinfrastructure for data-intensive science. This bundle follows the
[Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
v0.2: every document carries frontmatter, and every directory has an index like
this one.*

# Start here

* [About this documentation](https://docs.cyverse.org/about/overview/) - what CyVerse is, and where each audience should start
* [Deploying CyVerse from scratch](https://docs.cyverse.org/deployment/from-scratch/) - end-to-end walkthrough of a two-node deployment
* [Update log](https://docs.cyverse.org/log/) - what changed in this bundle, newest first
* [For AI agents](https://docs.cyverse.org/about/ai-agents/) - llms.txt, per-page Markdown, raw source on GitHub, and trust signals ([llms.txt](https://docs.cyverse.org/llms.txt), [llms-full.txt](https://docs.cyverse.org/llms-full.txt))
* [about/](https://docs.cyverse.org/about) - this documentation project: overview, contributing, agent guide

# Architecture

* [architecture/](https://docs.cyverse.org/architecture) - how the pieces fit together, what they need, and what talks to what
* [platform/](https://docs.cyverse.org/platform) - the products and services CyVerse offers its users

# Deployment

Ordered by dependency: nothing in a later phase can start before the phases above it.

* [deployment/](https://docs.cyverse.org/deployment) - the full deployment path, phase by phase
* [deployment/planning/](https://docs.cyverse.org/deployment/planning) - prerequisites and deployment tooling
* [deployment/01-foundation/](https://docs.cyverse.org/deployment/01-foundation) - HAProxy, PostgreSQL, RabbitMQ
* [deployment/02-databases/](https://docs.cyverse.org/deployment/02-databases) - per-service schemas and migrations
* [deployment/03-data-store/](https://docs.cyverse.org/deployment/03-data-store) - the iRODS zone and its DE integration
* [deployment/04-kubernetes/](https://docs.cyverse.org/deployment/04-kubernetes) - cluster, certificates, storage, ingress, registry
* [deployment/05-core-services/](https://docs.cyverse.org/deployment/05-core-services) - directory, authentication, search, messaging
* [deployment/06-applications/](https://docs.cyverse.org/deployment/06-applications) - Discovery Environment, VICE, User Portal
* [deployment/07-post-install/](https://docs.cyverse.org/deployment/07-post-install) - bootstrap, verification, troubleshooting

# Operating and integrating

* [operations/](https://docs.cyverse.org/operations) - running a deployment day to day
* [api/](https://docs.cyverse.org/api) - the Terrain API and its endpoints
* [development/](https://docs.cyverse.org/development) - contributing to CyVerse code
* [references/](https://docs.cyverse.org/references) - provenance notes for material this bundle derives from
