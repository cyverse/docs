---
type: Deployment Procedure
title: "HAProxy"
description: "The public entry point that terminates HTTPS and forwards to the cluster's Traefik node ports."
tags: [deployment, foundation, haproxy, networking]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: haproxy-docs
    resource: https://docs.haproxy.org/
    title: HAProxy documentation
    author: team:haproxy
---

# Role in the deployment

HAProxy is the single public entry point. It listens on `80/tcp` and `443/tcp`
and forwards to the node ports Traefik exposes inside the Kubernetes cluster.
Everything a user or a VICE client reaches — the DE, the User Portal, Keycloak,
interactive app URLs — arrives through it.

In the production US deployment, a second HAProxy fronts the Data Store's
access services (`data.cyverse.org`) as described in
[Data Store](../../platform/data-store.md); a pilot deployment usually runs
only the DE-facing instance.

# Order of operations

HAProxy is installed early, in phase 1, because it is a plain package install
with no dependencies. It is **configured** later, in phase 4, because its back
ends are the Traefik node ports, which do not exist until the cluster does.

1. **Phase 1** — install the `haproxy` package on the node in the
   `04_haproxy` inventory group.
2. **Phase 4** — after the cluster is up and Traefik is installed, apply the
   configuration:

   ```bash
   ansible-playbook -i /path/to/inventory --tags haproxy kubernetes.yml
   ```

Sizing: 4 cores, 8 GB memory, no dedicated storage. See
[component inventory](../../architecture/component-inventory.md).

# What the configuration has to line up with

| HAProxy front end | Back end |
|-------------------|----------|
| `80/tcp` | Redirect to HTTPS |
| `443/tcp` | Traefik HTTPS node port, `31344/tcp` by default |
| (optional) plain HTTP passthrough | Traefik HTTP node port, `31343/tcp` by default |

Both node ports are configurable. If you change them in the Traefik values, the
HAProxy back end has to change with them — a mismatch here produces a
connection refused on the public address while every pod looks healthy.

# TLS

Certificates are issued in-cluster by cert-manager, and Traefik terminates TLS
for in-cluster routes. Where HAProxy terminates TLS itself, its certificate has
to be renewed alongside the cluster issuer, and its trust store has to contain
the CA that signed the back-end certificates — a private CA in a pilot means
adding that CA to the system bundle on the HAProxy host.

# Related

* [Network requirements](../../architecture/network-requirements.md)
* [Ingress](../04-kubernetes/ingress.md)
* [Deploying from scratch](../from-scratch.md#11-haproxy)
