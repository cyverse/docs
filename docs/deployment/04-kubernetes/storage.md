---
type: Deployment Procedure
title: "Storage"
description: "Providing persistent volumes to the cluster with Longhorn or OpenEBS."
tags: [deployment, kubernetes, storage, longhorn, openebs]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: longhorn-docs
    resource: https://longhorn.io/docs/
    title: Longhorn documentation
    author: team:longhorn
  - id: openebs-docs
    resource: https://openebs.io/docs
    title: OpenEBS documentation
    author: team:openebs
---

# What needs persistent volumes

Cluster storage is for the stateful services that run *inside* Kubernetes —
Keycloak, Redis, the search cluster, Harbor — not for user data. User data lives
in the [Data Store](../../platform/data-store.md) and reaches pods through the
[iRODS CSI driver](../05-core-services/irods-csi-driver.md), which is a separate
concern with a separate lifecycle.

Two provisioners are in use across CyVerse deployments:

| Provisioner | Status | Notes |
|-------------|--------|-------|
| Longhorn | Current | Used by the pilot; replicated block storage with a management UI |
| OpenEBS | Legacy | `openebs-hostpath` storage class in older deployments |

Both can be present in one cluster, and a migration between them is a
volume-by-volume exercise. Pick one for new work.

# Longhorn

```bash
ansible-playbook -i /path/to/inventory --tags longhorn kubernetes.yml
```

Longhorn replicates each volume across nodes, so on a two-node cluster set the
replica count to what the node count can actually satisfy — asking for three
replicas on two nodes leaves volumes permanently `Degraded`. It also needs
`open-iscsi` on every node; the `prep-nodes` tag installs it.

Verify:

```bash
kubectl -n longhorn-system get pods
kubectl get storageclass
```

# OpenEBS (legacy)

Older deployments provision `openebs-hostpath` volumes:

```bash
kubectl create ns openebs
kubectl -n openebs apply -f https://openebs.github.io/charts/openebs-operator.yaml
```

`openebs-hostpath` is node-local: a pod using one of these volumes is pinned to
the node holding the data, and the volume does not survive that node's loss. The
[Redis HA](../05-core-services/redis-ha.md) values in this bundle still name
`openebs-hostpath` as their storage class — change it to your Longhorn class in
a new deployment.

# Choosing a storage class per service

| Service | Guidance |
|---------|----------|
| Keycloak, Harbor | Replicated (Longhorn); losing this data means re-registering clients or re-pushing images |
| Redis | Replicated preferred; Redis HA replicates at the application layer too |
| Search cluster | Replicated, sized generously; reindexing is expensive but possible |
| Scratch and caches | Node-local is fine |

# Related

* [Cluster](./cluster.md)
* [iRODS CSI driver](../05-core-services/irods-csi-driver.md)
* [Namespaces](../../architecture/namespaces.md)
