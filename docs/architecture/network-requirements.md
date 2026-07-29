---
type: Reference
title: "Network requirements"
description: "Every port a CyVerse deployment needs open, who needs to reach it, and why."
tags: [architecture, networking, firewall, ports]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
---

# How to read this

Open these before installing anything. A closed port in this table shows up
later as a timeout in an Ansible task or a pod stuck in `CrashLoopBackOff`,
several phases away from the cause.

Audience columns use three scopes:

* **Public** — reachable from anywhere users or clients connect from.
* **Internal** — between deployment nodes only.
* **Admin** — from administrator subnets only, never public.

# Public

| Port | Protocol | Service | Notes |
|------|----------|---------|-------|
| 80 | TCP | HAProxy → DE | Redirects to HTTPS |
| 443 | TCP | HAProxy → DE and VICE | Must be reachable from everywhere the DE and VICE are used |

# Data transfer

| Port | Protocol | Service | Reachable from |
|------|----------|---------|----------------|
| 1247 | TCP | iRODS provider | Analysis nodes and any host transferring data in or out |
| 20000–20199 | TCP | iRODS parallel transfer range | Same as above |
| 20000–20199 | UDP | iRODS parallel transfer range | Same as above |

The port range is a configuration choice made at install time; if you narrow it,
narrow it in `server_config.json` and in the firewall together, or transfers
stall after the control connection succeeds.

# Backing services

| Port | Protocol | Service | Reachable from |
|------|----------|---------|----------------|
| 5432 | TCP | PostgreSQL | Both deployment nodes, the pod network, and admin hosts |
| 5672 | TCP | RabbitMQ (AMQP) | Analysis nodes and in-cluster services |
| 15672 | TCP | RabbitMQ management UI | Admin only |

The pod network is not knowable until the cluster exists; see
[phase 4.5](../deployment/from-scratch.md#45-let-the-pods-reach-postgresql) for
adding the pod CIDR to `pg_hba.conf` after the fact.

# Kubernetes

Control-plane and worker ports for a k0s cluster:

| Port | Protocol | Purpose | Scope |
|------|----------|---------|-------|
| 6443 | TCP | Kubernetes API server | Internal + admin |
| 9443 | TCP | k0s join API | Internal + admin |
| 2380 | TCP | etcd peer traffic | Internal |
| 10250 | TCP | Kubelet metrics | Internal + admin |
| 8132 | TCP | Konnectivity | Internal |
| 179 | TCP | BGP (Calico) | Internal |
| 4789 | UDP | VXLAN (Calico) | Internal |
| — | IP protocol 112 | VRRP (keepalived) | Internal |
| 31343 | TCP | Traefik HTTP node port | Internal, from HAProxy |
| 31344 | TCP | Traefik HTTPS node port | Internal, from HAProxy |

!!! note "Corrections to watch for"

    * **VRRP is not a TCP port.** keepalived uses IP protocol 112, so the
      firewall rule is a protocol rule, not a port rule. Deployments with a
      single control-plane node do not run keepalived at all and can drop it.
    * **BGP and VXLAN depend on the CNI configuration.** A single-node control
      plane with VXLAN encapsulation may not need `179/tcp`. Confirm against
      your own Calico configuration rather than opening it by default.
    * **The Traefik node ports are configurable.** If you change them, change
      the HAProxy back end to match.

# Outbound

| Destination | Protocol | Purpose |
|-------------|----------|---------|
| DNS provider API | HTTPS | Let's Encrypt DNS-01 challenge for cert-manager |
| Container registries | HTTPS | Pulling service and VICE images |
| Package repositories | HTTPS | OS, iRODS, and Helm chart installation |

Outbound HTTPS to the DNS API is optional in the sense that the deployment
comes up without it, and strongly recommended in the sense that certificate
renewal is otherwise a recurring manual chore.

# Related

* [Component inventory and sizing](./component-inventory.md)
* [Ingress deployment](../deployment/04-kubernetes/ingress.md)
* [Deploying from scratch](../deployment/from-scratch.md)
