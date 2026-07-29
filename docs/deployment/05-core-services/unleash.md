---
type: Deployment Procedure
title: "Unleash"
description: "Deploying the Unleash feature-flag service the DE reads toggles from."
tags: [deployment, core-services, unleash, feature-flags]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

# Role in the deployment

Unleash holds the DE's feature toggles, including the maintenance flag that puts
the DE into a read-only banner state. The DE reads it through the `Unleash`
section of the deployment configuration: base URL, API path, API token, and the
maintenance flag name.

# Prerequisites

* [Unleash database](../02-databases/unleash.md) created.
* The `Unleash` group variables filled in; see
  [cluster resources](../04-kubernetes/resources.md).

# Deploy

From the [cluster resources](../04-kubernetes/resources.md) checkout,
substituting the namespace the DE runs in:

```bash
kubectl apply -f resources/deployments/unleash.yml -n <NAMESPACE>
```

# Verify

```bash
kubectl -n <NAMESPACE> get pods -l app=unleash
kubectl -n <NAMESPACE> logs deploy/unleash --tail=50
```

Unleash runs its own schema migrations at startup, so the first start after a
version bump takes longer than usual. A pod that restarts repeatedly on first
boot is normally failing to reach the database.

# Related

* [Unleash database](../02-databases/unleash.md)
* [Cluster resources](../04-kubernetes/resources.md)
