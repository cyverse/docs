---
type: Deployment Procedure
title: "iRODS CSI driver"
description: "Installing the iRODS Container Storage Interface driver that mounts Data Store paths into pods."
tags: [deployment, core-services, irods, storage, csi]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---
# Role in the deployment

The [iRODS CSI driver](https://github.com/cyverse/irods-csi-driver) implements the
Container Storage Interface so that Kubernetes can mount Data Store paths straight
into pods. It is how a VICE analysis sees the user's home collection as a
filesystem instead of having to stage data in and out.

It is a separate concern from [cluster storage](../04-kubernetes/storage.md):
Longhorn and OpenEBS provide volumes to stateful cluster services, while this
driver provides user data to analyses.

# Prerequisites

* A running [iRODS provider](../03-data-store/irods-provider.md) reachable from
  the cluster on `1247/tcp`.
* The `de-irods` account from
  [DE integration](../03-data-store/de-integration.md), or another iRODS admin
  account for the driver to proxy through.

# Values

Create a `values.yaml`. It contains an iRODS admin password, so keep it in your
private inventory.

```yaml
globalConfig:
  secret:
    stringData:
      client: "irodsfuse"
      host: <IRODS-SERVER-HOST>
      port: "1247"
      zone: "<IRODS_ZONE>"
      user: <IRODS-ADMIN-USER>
      password: <GENERATED_SECRET>
      retainData: "false"
      enforceProxyAccess: "true"
      mountPathWhitelist: "/<IRODS_ZONE>/home"
nodeService:
  irodsPool:
    extraArgs:
      - --cache_size_max=10737418240
      - --cache_root=/irodsfs_pool_cache
      - '--cache_timeout_settings=[{"path":"/","timeout":"-1ns","inherit":false},{"path":"/<IRODS_ZONE>","timeout":"-1ns","inherit":false},{"path":"/<IRODS_ZONE>/home","timeout":"5m","inherit":false},{"path":"/<IRODS_ZONE>/home/shared","timeout":"5m","inherit":true}]'

```

Two settings deserve attention:

* **`enforceProxyAccess: "true"`** makes the driver act on behalf of the
  requesting user rather than as the admin account. Leave it on — with it off, any
  pod that can mount a volume reads the zone with admin rights.
* **`mountPathWhitelist`** bounds what can be mounted at all. Keep it as narrow as
  your analyses allow.

# Deploy

```bash
# Add the Helm repository.
helm repo add irods-csi-driver-repo https://cyverse.github.io/irods-csi-driver-helm/

# Update the local repository caches.
helm repo update

# create namespace
kubectl create namespace irods-csi-driver

# install csi-driver
# make sure to edit values.yaml
helm install -n irods-csi-driver irods-csi-driver irods-csi-driver-repo/irods-csi-driver -f ./values.yaml

# or upgrade
helm upgrade -n irods-csi-driver irods-csi-driver irods-csi-driver-repo/irods-csi-driver -f ./values.yaml

```

# Verify

```bash
kubectl -n irods-csi-driver get pods
kubectl get csidrivers
```

# Upgrading

An upgrade is disruptive: running interactive analyses hold volumes provisioned by
the current driver, so they have to be stopped and their claims removed first.
Schedule it, and warn users.

```bash
# update helm repo
helm repo update

# delete the pvc
kubectl delete pvc -l app-type=interactive -n vice-apps

# uninstall the irods-csi-driver
helm uninstall irods-csi-driver -n irods-csi-driver

# delete all the vice-apps deployments
## see below for the content of this file
./nuke-vice-analysis.sh $(kubectl get deployments -n vice-apps -l app-type=interactive -o name)

# install again
helm install -n irods-csi-driver irods-csi-driver irods-csi-driver-repo/irods-csi-driver -f values.yaml
```

## Pinning a version

```bash
helm install -n irods-csi-driver irods-csi-driver \
    --version <CHART_VERSION> irods-csi-driver-repo/irods-csi-driver -f values.yaml
```

Pin the chart version in a production deployment. `helm search repo
irods-csi-driver-repo` lists what is available.

!!! warning "Configuration change after 0.8.7"

    In chart versions above 0.8.7, the `user_config.yaml` handling changed: the
    `--cache_root` and `--temp_root` flags must be removed if you were passing
    them. Leaving them in place makes the node service fail to start.

# nuke-vice-analysis.sh

Used by the upgrade procedure above to tear down interactive analyses along with
the resources `app-exposer` created for them.

```sh
function delete_resources() {
    local external_id="$1"
    kubectl -n vice-apps delete deployment "${external_id}"
    kubectl -n vice-apps delete service "vice-${external_id}"
    kubectl -n vice-apps delete ingress "${external_id}"
    kubectl -n vice-apps delete configmap "excludes-file-${external_id}"
    kubectl -n vice-apps delete configmap "input-path-list-${external_id}"
}

function remove_deployment_prefix() {
    local external_id="$1"
    echo -n "$external_id" | sed 's;^deployment.apps/;;'
}

# Iterate over all arguments on the command line.
for id in "$@"; do
    delete_resources $(remove_deployment_prefix "$id")
done
```

# Related

* [iRODS provider](../03-data-store/irods-provider.md)
* [VICE](../06-applications/vice.md)
* [Cluster storage](../04-kubernetes/storage.md)
