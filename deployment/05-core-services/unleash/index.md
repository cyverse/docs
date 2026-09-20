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

* [Unleash database](https://docs.cyverse.org/deployment/02-databases/unleash/) created.
* The `Unleash` group variables filled in; see
  [cluster resources](https://docs.cyverse.org/deployment/04-kubernetes/resources/).

# Deploy

From the [cluster resources](https://docs.cyverse.org/deployment/04-kubernetes/resources/) checkout,
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

* [Unleash database](https://docs.cyverse.org/deployment/02-databases/unleash/)
* [Cluster resources](https://docs.cyverse.org/deployment/04-kubernetes/resources/)
