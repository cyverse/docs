# Discovery Environment

!!! success "Prerequisites"

    * Make sure you have the `harbor-registry-credentials` secrets in your NAMESPACE, see also [k8s-resources](k8s-resources.md)

    * Make sure you have `de-nginx-tls` secret created, also see [k8s-resources](k8s-resources.md)

    * Make sure your Haproxy has the CA cert, Add `/docker-tugraz-data/ca/ca.pem` to `HAPROXY_DOMAIN`:/etc/ssl/certs/ca-bundle.crt`


**remember** `nginx` is proxying all the services to the DE in CyVerse.

* Use the manifest files from the [k8s-resources](k8s-resources.md)

### Change hardcoded

If you are using a diffrent domain instead of `cyverse.tugraz.at`, e.g. `cyverse.at` - change these two files.

**/k8s-resources/resources/kustomize/de-nginx/base/nginx.conf**

```diff
- server_name ~^[^.]+[.]cyverse[.]tugraz[.]at$;
+ server_name ~^[^.]+[.]cyverse[.]at$;
```

**k8s-resources/resources/kustomize/de-nginx/base/kustomization.yaml**

```diff
- namespace: prod
+ namespace: discover
```

### Deploy

```bash
## For prod env
kubectl apply -k resources/kustomize/de-nginx/overlays/prod/ -n prod
kubectl apply -f resources/services/tugraz.yml -n prod

## For discover env
# kubectl apply -k resources/kustomize/de-nginx/overlays/prod/ -n discover
# kubectl apply -f resources/services/tugraz.yml -n discover
```
