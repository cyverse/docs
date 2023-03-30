# iRODS CSI Driver

iRODS Container Storage Interface (CSI) Driver implements the CSI Specification to provide container orchestration engines (like Kubernetes) iRODS access.

!!! success "Prerequisites"



Before we install `irods-csi-driver`, we need to create a `values.yaml` file.

**Change the values accordingly and save the file as `values.yaml`.**

```yaml
globalConfig:
  secret:
    stringData:
      client: "irodsfuse"
      host: <IRODS-SERVER-HOST>
      port: "1247"
      zone: "TUG"
      user: <IRODS-ADMIN-USER>
      password: <PASSWORD>
      retainData: "false"
      enforceProxyAccess: "true"
      mountPathWhitelist: "/TUG/home"
nodeService:
  irodsPool:
    extraArgs:
      - --cache_size_max=10737418240
      - --cache_root=/irodsfs_pool_cache
      - '--cache_timeout_settings=[{"path":"/","timeout":"-1ns","inherit":false},{"path":"/TUG","timeout":"-1ns","inherit":false},{"path":"/TUG/home","timeout":"5m","inherit":false},{"path":"/TUG/home/shared","timeout":"5m","inherit":true}]'

```

## Deploy

we use helm to deploy irods-csi-driver.

```bash
# Add the Helm repository.
helm repo add irods-csi-driver-repo https://cyverse.github.io/irods-csi-driver-helm/

# Update the local repository caches.
helm repo update

# create namespace
kubectl create namespace irods-csi-driver

# install csi-driver
# make sure to edit values-cyverse.at.yaml
helm install -n irods-csi-driver irods-csi-driver irods-csi-driver-repo/irods-csi-driver -f ./values.yaml

# or upgrade
helm upgrade -n irods-csi-driver irods-csi-driver irods-csi-driver-repo/irods-csi-driver -f ./values.yaml

```

## Upgrading to a newer version

When we want to upgrade the `irods-csi-driver` to a newer version, we need to stop all running vice-apps and delete all the pvcs.

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

## install specific version
```bash
helm install -n irods-csi-driver irods-csi-driver --version 0.8.7 irods-csi-driver-repo/irods-csi-driver -f values.yaml

## try the latest
helm install -n irods-csi-driver irods-csi-driver --version 0.9.2 irods-csi-driver-repo/irods-csi-driver -f values.yaml
```

## NOTE
With `irods-csi-driver` **version > 0.8.7**, there’s a small change on the configuration file `user_config.yaml` for driver installation.
**You will need to delete --cache_root and --temp_root flags if you used it.**



# nuke-vice-analysis.sh

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