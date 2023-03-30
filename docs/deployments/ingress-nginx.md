# ingress-nginx

!!! success "Prerequisites"


**ingress-nginx** is used to give vice-apps ingresses, using nodeport and what not.


## deploy

The deployment manifests are in [k8s-resources](k8s-resources.md).

### (optional) changing the env

Modify file `resources/kustomize/ingress-nginx/overlays/prod/args.yaml`, to change the namespace.

```diff
- --default-backend-service=prod/vice-default-backend
+ --default-backend-service=discover/vice-default-backend
```

### Deploy kustomize

This script will create a namespace `ingress-nginx`.

```bash
kubectl apply -k resources/kustomize/ingress-nginx/overlays/prod
```


