---
type: Reference
title: "Prerequisites"
description: "Hardware, skills, tooling, and access you need in place before deploying CyVerse."
tags: [deployment, planning, prerequisites]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

# Before you start

CyVerse is a full-stack cyberinfrastructure: a data platform, an authentication
stack, a Kubernetes cluster, and a dozen services on top. Deploying it is a
project, not an afternoon. This document is the readiness check; the ordered
procedure is in [deployment](../index.md), and the narrative walkthrough of a
real two-node build is [deploying from scratch](../from-scratch.md).

# Infrastructure

!!! success "Hardware or cloud"

    * Bare metal, an OpenStack cloud, or a commercial provider.
    * Enough capacity for the components in
      [component inventory](../../architecture/component-inventory.md). The
      smallest useful deployment is two well-provisioned servers; a production
      deployment is a cluster.
    * Storage for the iRODS vault, sized to the science rather than to a
      recommendation, plus persistent volumes for the cluster's stateful
      services.
    * Public IP addresses, and DNS you can create records in — including a
      wildcard for VICE.
    * API access to your DNS provider, for automated certificate issuance.

!!! success "Network"

    * The ports in
      [network requirements](../../architecture/network-requirements.md), open
      before you begin rather than debugged later.
    * Institutional firewall changes agreed in advance. On a university network
      this is usually the longest lead time in the whole project.
    * Experience operating in a
      [Science DMZ](https://en.wikipedia.org/wiki/Science_DMZ_Network_Architecture)
      architecture helps: high-throughput data transfer and segmented services
      are exactly what this stack needs.

# Skills

* Linux system administration, including filesystem permissions and systemd.
* Kubernetes cluster operation — not just `kubectl apply`, but reading events,
  debugging pod networking, and understanding storage classes.
* Ansible for configuration management.
* PostgreSQL administration and tuning.
* Container fundamentals and registry management.
* DNS, TLS, load balancing, and ingress.
* iRODS concepts: zones, resources, vaults, and the catalog.

Nobody has all of these in equal measure. What matters is that the deployment
team as a whole does, and that whoever runs it can tell a configuration error
from a network error from a policy error.

# Tooling

On the workstation you deploy from:

| Tool | Used for |
|------|----------|
| `ansible` | Everything the playbooks do |
| `kubectl`, `helm` | Cluster operations, chart installs |
| `k0sctl` | Creating and upgrading the cluster |
| `git` | The deployment and inventory repositories |
| `psql` | Database work, including bootstrap steps |
| `uv` | The `appei` app import and export tool |
| `gomplate` | Rendering the configuration and secret templates |
| `skaffold` | Building and deploying individual services during development |

Setup instructions: [Ansible](./ansible.md), [Docker](./docker.md).

# Access

* `ssh` to every node, key-based, with `sudo`.
* A **private** git repository for the inventory, `group_vars`, and generated key
  material. Nothing in this list belongs in a public repository.
* A container registry you control — see [Harbor](../04-kubernetes/harbor.md).
* Administrative access to the DNS zone.

!!! danger "Secrets discipline, decided up front"

    A CyVerse deployment generates a lot of secrets: database passwords, an iRODS
    zone key, a negotiation key, a password salt, eight Keycloak client secrets,
    GPG and PEM signing keys, registry credentials, and DNS API tokens.

    Decide where they live before you generate the first one. The pattern that
    works is: generate at install time, store in the private inventory repository,
    template into place with Ansible, and never paste into a shell.

# Temperament

Distributed systems fail in layers, and this one has many. The single most useful
habit is to verify each phase before starting the next — which is what the phase
order in [deployment](../index.md) is for, and why
[verification](../07-post-install/verification.md) exists as its own document.

# Next

* [Deployment overview and order](../index.md)
* [Deploying from scratch](../from-scratch.md)
* [Component inventory and sizing](../../architecture/component-inventory.md)
* [Network requirements](../../architecture/network-requirements.md)
