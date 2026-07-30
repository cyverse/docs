---
type: Deployment Procedure
title: "Harbor"
description: "Deploying the Harbor container registry that holds CyVerse service and tool images."
tags: [deployment, kubernetes, harbor, registry]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: harbor-docs
    resource: https://goharbor.io/docs/
    title: Harbor documentation
    author: team:harbor
---

# Role in the deployment

Harbor is the container registry for CyVerse service images and integrated tool
images. The public CyVerse instance is at
[harbor.cyverse.org](https://harbor.cyverse.org/); a self-contained deployment
runs its own so that image pulls do not depend on another site's registry.

Install it after [storage](./storage.md) — Harbor is stateful, and its registry
and database volumes need a working storage class before the chart will come up.

# Install

```bash
ansible-playbook -i /path/to/inventory --tags harbor kubernetes.yml
```

# What consumes it

| Consumer | How it authenticates |
|----------|---------------------|
| DE services | `harbor-registry-credentials` secret in the DE namespace |
| VICE apps | `vice-image-pull-secret` in the `vice-apps` namespace |
| Image cache | Pre-pulls frequently used VICE images onto workers |
| Tool integration | Users and administrators push tool images |

Both pull secrets are created as part of
[cluster resources](./resources.md) and
[VICE deployment](../06-applications/vice.md). A missing pull secret shows up as
`ImagePullBackOff` on an image that exists and is readable by hand — check the
secret before the registry.

The deployment configuration also holds a Harbor robot account (the `Harbor`
section of the group variables: URL, robot name, robot secret) used by automated
image operations. Treat the robot secret like any other deployment secret.

# Related

* [Cluster resources](./resources.md)
* [VICE](../06-applications/vice.md)
* [Storage](./storage.md)
