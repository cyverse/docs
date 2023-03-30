# vice apps

!!! success "Prerequisites"


## Create namespace
```bash
kubectl create ns vice-apps
```

## Create a secret on namespace vice-apps `vice-image-pull-secret`

This secret has the `harbor.org` which allows the pod to pull docker images.

```bash
kubectl create secret generic vice-image-pull-secret \
    --from-file=.dockerconfigjson=/root/.docker/config.json \
    --type=kubernetes.io/dockerconfigjson -n vice-apps
```

## Create serviceAccounts

```bash
## edit the namespace if you are running in a diffrent env
## vi /k8s-resources/resources/serviceaccounts/app-exposer.yml
kubectl apply -f /k8s-resources/resources/serviceaccounts/app-exposer.yml
kubectl apply -f /k8s-resources/resources/serviceaccounts/vice-app-runner.yml
```

## Apply clusterrolebindings

```bash
## edit the namespace if you are running in a diffrent env
## vi /k8s-resources/resources/clusterrolebindings/app-exposer.yml
kubectl apply -f /k8s-resources/resources/clusterrolebindings/app-exposer.yml
```

## Apply networkpolicies

To apply networkpolicies we need to edit the file `/k8s-resources/resources/networkpolicies/vice-apps.yml`, and add all the worker nodes, and master nodes, to it if we are using a diffrent **env** rather than
**prod**.

**e.g.**

```diff

-        except:
         - ********
         - ********

+        except:
         - 10.0.10.0/24          # k8s master CIDR
         - ****************/32   # c1 
         - ****************/32   # w1 
         - ***************/32    # w2 
         - ***************/32    # w3 
         - **************/32     # w4
         - *************/32      # w5 
         - ************/32       # vice-w1 
```

**Run policy**

```bash
kubectl apply -f /k8s-resources/resources/networkpolicies/vice-apps.yml
```

## Apply roles

```bash
kubectl apply -f /k8s-resources/resources/roles/vice-apps.yml
```

## Create porklock-config secrert for irods

### Create irods-config.properties

Create a file `irods-config.properties` and add the values.

```properties
porklock.irods-home=
porklock.irods-user=
porklock.irods-pass=
porklock.irods-host=
porklock.irods-port=
porklock.irods-zone=
porklock.irods-resc=
```

### Create secret from file

```bash
kubectl -n vice-apps create secret generic porklock-config --from-file=irods-config.properties
```

## Restart the services
```bash
kubectl rollout restart apps app-exposer templeton-incremental templeton-periodic -n NAMESPACE
```

## Install/configure ingress-nginx

For installing and configuring the **ingress-nginx** have a look at [ingress-nginx](ingress-nginx.md)
