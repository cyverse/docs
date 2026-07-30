---
okf_version: "0.2"
---

![](assets/cyverse_logo_2022.svg)

*Technical documentation for deploying and operating CyVerse — the open-source
cyberinfrastructure for data-intensive science. This bundle follows the
[Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
v0.2: every document carries frontmatter, and every directory has an index like
this one.*

# Start here

* [About this documentation](about.md) - what CyVerse is, and where each audience should start
* [Deploying CyVerse from scratch](deployment/from-scratch.md) - end-to-end walkthrough of a two-node deployment
* [Update log](log.md) - what changed in this bundle, newest first

# Architecture

* [architecture/](architecture/) - how the pieces fit together, what they need, and what talks to what
* [platform/](platform/) - the products and services CyVerse offers its users

# Deployment

Ordered by dependency: nothing in a later phase can start before the phases above it.

* [deployment/](deployment/) - the full deployment path, phase by phase
* [deployment/planning/](deployment/planning/) - prerequisites and deployment tooling
* [deployment/01-foundation/](deployment/01-foundation/) - HAProxy, PostgreSQL, RabbitMQ
* [deployment/02-databases/](deployment/02-databases/) - per-service schemas and migrations
* [deployment/03-data-store/](deployment/03-data-store/) - the iRODS zone and its DE integration
* [deployment/04-kubernetes/](deployment/04-kubernetes/) - cluster, certificates, storage, ingress, registry
* [deployment/05-core-services/](deployment/05-core-services/) - directory, authentication, search, messaging
* [deployment/06-applications/](deployment/06-applications/) - Discovery Environment, VICE, User Portal
* [deployment/07-post-install/](deployment/07-post-install/) - bootstrap, verification, troubleshooting

# Operating and integrating

* [operations/](operations/) - running a deployment day to day
* [api/](api/) - the Terrain API and its endpoints
* [development/](development/) - contributing to CyVerse code
* [references/](references/) - provenance notes for material this bundle derives from
