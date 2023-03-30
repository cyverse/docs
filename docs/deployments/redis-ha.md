# Redis HA

In this Document we will cover installing the **Redis Server** and **Redis Haproxy**, due to the `k8s-resources` configurations both would be installed and configured inside the **prod** namespace.

!!! success "Prerequisites"

    * Make sure to create a **values.yaml** and replace add your credentials


**values.yaml**
Overrides the PersistentVolume, adds secrets and uses **openebs** as storage class.

```yaml
## replicas number for each component
replicas: 3

persistentVolume:
  enabled: true
  ## redis-ha data Persistent Volume Storage Class
  ## If defined, storageClassName: <storageClass>
  ## If set to "-", storageClassName: "", which disables dynamic provisioning
  ## If undefined (the default) or set to null, no storageClassName spec is
  ##   set, choosing the default provisioner.  (gp2 on AWS, standard on
  ##   GKE, AWS & OpenStack)
  ##
  storageClass: openebs-hostpath

## Sentinel specific configuration options
sentinel:
  auth: true
  authkey: <AUTH-KEY>
  password: <PASSWORD>

## Redis specific configuration options
auth: true
authkey: <AUTH-KEY>
redisPassword: <PASSWORD>
```

## Redis Server & Sentinel

```bash
# add helm repo
helm repo add dandydev https://dandydeveloper.github.io/charts
helm repo update

## deploy redis servers for prod env
helm upgrade --install --namespace prod redis-ha dandydev/redis-ha --values values.yaml

## deploy redis server for discover env
# helm upgrade --install --namespace discover redis-ha dandydev/redis-ha --values values.yaml
```

# Redis Haproxy

## Preq

* **Make sure to load the configs/secrets** see also [k8s-resources](k8s-resources.md)

## Deploy redis-haproxy


```bash
## deploy for prod env
kubectl apply -n prod -f resources/deployments/redis-haproxy.yml

## deploy for discover env
# kubectl apply -n discover -f resources/deployments/redis-haproxy.yml
```
