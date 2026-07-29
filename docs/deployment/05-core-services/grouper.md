---
type: Deployment Procedure
title: "Grouper"
description: "Deploying the Grouper loader and web services for group management."
tags: [deployment, core-services, grouper, groups]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

# Role in the deployment

[Internet2 Grouper](https://www.internet2.edu/products-services/trust-identity/grouper/)
manages the groups CyVerse authorizes against — the DE reads group membership
through it rather than querying LDAP directly. Two deployments make it up:

| Deployment | Job |
|------------|-----|
| `grouper-loader` | Syncs subjects and groups from the directory on a schedule |
| `grouper-ws` | Web services the DE queries at request time |

# Prerequisites

* [Grouper database](../02-databases/grouper.md) created.
* [OpenLDAP](./openldap.md) running, with the `ou=Groups` branch populated.
* The `Grouper` section of the deployment group variables filled in — loader URI
  and credentials, web service password, morph string, folder name prefix, and
  subject source configuration. See
  [cluster resources](../04-kubernetes/resources.md).

# Deploy

Ansible deploys both parts along with the other core services:

```bash
ansible-playbook -i /path/to/inventory \
  --tags=feature-discovery,image-cache,grouper kubernetes.yml
```

To apply the manifests directly instead — from the
[cluster resources](../04-kubernetes/resources.md) checkout, substituting the
namespace the DE runs in:

```bash
kubectl apply -f resources/deployments/grouper-loader.yml -n <NAMESPACE>
kubectl apply -f resources/deployments/grouper-ws.yml     -n <NAMESPACE>
```

# Verify

```bash
kubectl -n <NAMESPACE> get pods -l app=grouper-ws
kubectl -n <NAMESPACE> logs deploy/grouper-loader --tail=100
```

The loader logs each sync. A loader that starts and then idles without syncing
usually cannot reach either the database or the directory; check both before
looking at Grouper's own configuration.

# Related

* [Grouper database](../02-databases/grouper.md)
* [OpenLDAP](./openldap.md)
* [Keycloak](./keycloak.md)
