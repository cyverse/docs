---
type: Deployment Procedure
title: "cert-manager"
description: "Installing cert-manager and the cluster issuers that mint TLS certificates for every CyVerse route."
tags: [deployment, kubernetes, tls, cert-manager]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: cert-manager-docs
    resource: https://cert-manager.io/docs/
    title: cert-manager documentation
    author: team:cert-manager
---

# Why it comes first

cert-manager is installed immediately after the cluster and before any ingress,
because ingress routes reference certificates by secret name. Install it the
other way round and every route comes up without TLS and has to be reconciled
later.

# Install

```bash
ansible-playbook -i /path/to/inventory --tags cert-manager kubernetes.yml
ansible-playbook -i /path/to/inventory --tags cert-issuers kubernetes.yml
```

The two tags are deliberately separate: the first installs the controller and
its CRDs, the second creates the `ClusterIssuer` objects. The issuers cannot be
created until the CRDs exist, so run them in this order and let the first
finish.

# Issuers

A CyVerse deployment normally defines two issuers:

| Issuer | Use |
|--------|-----|
| Let's Encrypt staging | Validating the issuance path without burning rate limits |
| Let's Encrypt production | The certificates users see |

VICE gives interactive apps hostnames under `*.vice.<BASE_DOMAIN>`, which means
a **wildcard** certificate, which means the **DNS-01** challenge — HTTP-01
cannot satisfy a wildcard. That is why
[network requirements](../../architecture/network-requirements.md) calls for
outbound HTTPS to your DNS provider's API: the issuer needs credentials for a
DNS provider it can write TXT records through.

Those credentials are a secret. They go in your private inventory and reach the
cluster as a Kubernetes secret referenced by the issuer, never in a manifest
committed here.

# Verify

```bash
kubectl get clusterissuers
kubectl get certificates -A
kubectl describe certificate <name> -n <namespace>
```

A certificate stuck in `False` / `Issuing` is almost always the challenge, not
the certificate: check the `Order` and `Challenge` objects it owns, and confirm
the DNS credentials and outbound access.

Validate against the staging issuer first. Production Let's Encrypt rate limits
are per registered domain per week, and a misconfigured wildcard can exhaust
them quickly.

# Related

* [Ingress](./ingress.md)
* [HAProxy](../01-foundation/haproxy.md)
* [Deploying from scratch](../from-scratch.md#44-cluster-add-ons-in-order)
