---
type: Deployment Procedure
title: "VICE"
description: "Preparing the vice-apps namespace, service accounts, network policies, and secrets for interactive apps."
tags: [deployment, applications, vice, interactive]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
---

# What VICE needs

VICE (the Visual Interactive Computing Environment) runs user-supplied
interactive containers — JupyterLab, RStudio, Shiny, Cloud Shell — inside the
cluster, each with its own ingress and its own mount of the user's Data Store
home. That means it needs more isolation than any other part of the deployment:
a dedicated namespace, dedicated service accounts, and a network policy that
keeps user containers away from cluster infrastructure.

Interactive analyses run on the workers in the `k8s_vice_workers` inventory
group, kept separate from the DE service set so a runaway analysis cannot starve
the platform.

# Namespace and image pulls

```bash
kubectl create ns vice-apps
```

Interactive app images come from [Harbor](../04-kubernetes/harbor.md), so the
namespace needs a pull secret:

```bash
kubectl create secret generic vice-image-pull-secret \
    --from-file=.dockerconfigjson=<PATH_TO_DOCKER_CONFIG_JSON> \
    --type=kubernetes.io/dockerconfigjson -n vice-apps
```

Point `--from-file` at a Docker config containing **only** the registry
credentials VICE needs. A personal `~/.docker/config.json` often holds
credentials for several registries, and everything in the file becomes readable
to anything that can read the secret.

# Service accounts, bindings, and roles

From the [cluster resources](../04-kubernetes/resources.md) checkout. Check the
namespace in each manifest before applying if you are not deploying into the
default environment:

```bash
kubectl apply -f resources/serviceaccounts/app-exposer.yml
kubectl apply -f resources/serviceaccounts/vice-app-runner.yml
kubectl apply -f resources/clusterrolebindings/app-exposer.yml
kubectl apply -f resources/roles/vice-apps.yml
```

`app-exposer` creates and tears down per-analysis deployments, services, and
ingresses; `vice-app-runner` is the identity the user's container itself runs as.

# Network policy

`resources/networkpolicies/vice-apps.yml` is what stops interactive containers
from reaching cluster infrastructure. It allows egress broadly and then subtracts
the addresses user containers must not reach — the control plane and the worker
nodes themselves:

```yaml
        except:
          - <K8S_CONTROL_PLANE_CIDR>   # control plane
          - <NODE_IP>/32               # one entry per node
```

Add an entry for **every** node in the cluster, control plane and workers alike,
and revisit the list whenever a node is added. A node missing from the exception
list is a node that user containers can reach directly.

```bash
kubectl apply -f resources/networkpolicies/vice-apps.yml
```

# Data transfer credentials

Analyses stage data in and out of the Data Store with `porklock`, which reads its
configuration from a secret in the `vice-apps` namespace. Create
`irods-config.properties`:

```properties
porklock.irods-home=/<IRODS_ZONE>/home
porklock.irods-user=de-irods
porklock.irods-pass=<GENERATED_SECRET>
porklock.irods-host=<IRODS_HOST>
porklock.irods-port=1247
porklock.irods-zone=<IRODS_ZONE>
porklock.irods-resc=<IRODS_DEFAULT_RESOURCE>
```

```bash
kubectl -n vice-apps create secret generic porklock-config \
    --from-file=irods-config.properties
```

Use the `de-irods` credentials from
[iRODS integration](../03-data-store/de-integration.md), then delete the local
file — it holds an iRODS admin password in plain text.

# Ingress

Interactive analyses get per-analysis hostnames under `*.vice.<BASE_DOMAIN>`,
served today through ingress-nginx. See [ingress](../04-kubernetes/ingress.md)
for the controller and [cert-manager](../04-kubernetes/cert-manager.md) for the
wildcard certificate those hostnames need.

# Apply configuration changes

After changing VICE configuration or secrets, restart the services that read
them:

```bash
kubectl rollout restart deployment \
    app-exposer templeton-incremental templeton-periodic -n <NAMESPACE>
```

# Before analyses will launch

A VICE **operator** has to be registered through the DE admin UI, which is a
post-install step rather than a deployment step:
[register a VICE operator](../07-post-install/bootstrap.md#register-a-vice-operator).

# Related

* [iRODS CSI driver](../05-core-services/irods-csi-driver.md) — how home
  directories are mounted into analysis pods
* [Namespaces](../../architecture/namespaces.md)
* [Discovery Environment](./discovery-environment.md)
