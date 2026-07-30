---
type: Reference
title: "Pilot CyVerse deployment record"
description: "Provenance note for the internal pilot deployment write-up that the from-scratch runbook and sizing tables derive from."
tags: [references, provenance, pilot]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

# What this is

Several documents in this bundle derive from an internal write-up of a
two-node CyVerse pilot deployment: a component and sizing list, a port list, an
ordered set of installation steps, and a short list of fixes found during the
install.

That write-up is not reproduced here. It records one site's hostnames, zone
name, LDAP DNs, realm and client names, service account names, and the shape of
its secrets. Publishing it verbatim would leak site detail without helping
anyone deploy, so the material was rewritten with placeholders and generalized
one step away from the pilot.

# What was derived from it

| Document | What it took |
|----------|--------------|
| [Deploying CyVerse from scratch](../deployment/from-scratch.md) | The phase order and every installation step |
| [Component inventory and sizing](../architecture/component-inventory.md) | Component list, per-component requirements, node allocation, dependency graph |
| [Network requirements](../architecture/network-requirements.md) | The port list |
| [PostgreSQL](../deployment/01-foundation/postgresql.md) | Kernel and `postgresql.conf` tuning |
| [iRODS provider](../deployment/03-data-store/irods-provider.md) | Installer answers, policy installation, runtime initialization |
| [Keycloak](../deployment/05-core-services/keycloak.md) | Realm, LDAP federation, mapper, role, and client configuration |
| [Troubleshooting](../deployment/07-post-install/troubleshooting.md) | The fixes found during the install |

# How it was transformed

* **Hostnames and domains** became `core-1`, `analysis-1`, and
  `<BASE_DOMAIN>`.
* **Realm, client, and account names** became `<SITE>`-prefixed placeholders.
* **The iRODS zone name** became `<IRODS_ZONE>`.
* **LDAP DNs** became `<LDAP_BASE_DN>`.
* **Passwords, salts, zone keys, and negotiation keys** became
  `<GENERATED_SECRET>`, with instructions to generate a fresh value per install
  rather than any hint of the original.
* **A pointer to an example configuration file shared in a chat channel**
  became a sanitized example checked in beside the procedure.
* **Individual names** in estimate annotations became the team.
* **Errors were corrected rather than copied.** The corrections are called out
  where they appear — an rsyslog path, a shell command, a protocol number, and
  a rounding inconsistency in the sizing totals.

# Freshness

The pilot deployment used iRODS 4.3.3, PostgreSQL 16, and k0s-managed
Kubernetes. Version-specific steps are marked where they matter. Treat the
upstream project documentation, not this bundle, as authoritative for anything
version-dependent.
