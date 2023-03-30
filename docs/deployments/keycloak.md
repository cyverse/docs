# Keycloak

!!! success "Prerequisites"

### configure database 

To setup and configure database please have a look at [keycloak database](../database/keycloak-db.md).


### create namsespace

```bash
# create ns
kubectl create ns keycloak
```

### Create required secrets & configmap

Keycloak deployment requires secrets and configmaps, which can be done via a `kustomization.yaml` file, please see below for an example of this template:

```yaml
secretGenerator:
- name: dbuser      # Database
  literals:
  - username=<USERNAME>
  - password=<PASSWORD>
- name: kcadmin     # keyclaok
  literals:
  - username=<USERNAME>
  - password=<PASSWORD>
configMapGenerator:
- name: keycloak-config
  literals:
  - KEYCLOAK_HOSTNAME=keycloak.example.com
  - KEYCLOAK_LOGLEVEL=INFO
  - DB_VENDOR=postgres
  - DB_ADDR=<DATABASE_HOST>
  - DB_PORT=5432
  - PROXY_ADDRESS_FORWARDING=true
  - JDBC_PARAMS=connectTimeout=21600
  - JAVA_OPTS=-server
    -Xms4096m
    -Xmx8192m
    -XX:MetaspaceSize=96m
    -XX:MaxMetaspaceSize=256m
    -Djboss.modules.system.pkgs=org.jboss.byteman
    -Djava.awt.headless=true
    -Dkeycloak.profile.feature.token_exchange=enabled
    -Djava.security.egd=file:/dev/urandom
namespace: keycloak
resources:
- deployment.yaml
- service.yaml
generatorOptions:
  disableNameSuffixHash: true
```

* Update the values of the `kustomization.yaml` file.
* place the `kustomization.yaml`, `deployment.yaml` and `service.yaml` inside a directory. e.g. `base`

### deployment & service YAML files:

**TODO:** find a way to add those files.

## Deploy

```bash
# apply kustomize
kubectl apply -k ./base/ -n keycloak
```
