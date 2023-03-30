# Grouper

!!! success "Prerequisites"

    * Make sure `grouper` database is setup, see also [grouper-db](../database/grouper-db.md)

## Deploy
Grouper consist of two deployments, `grouper-loader` & `grouper-ws`.
### grouper-loader

```bash
## Deploy for prod env
kubectl apply -f resources/deployments/grouper-loader.yml -n prod

## Deploy for discover env
# kubectl apply -f resources/deployments/grouper-loader.yml -n discover
```

### grouper-ws

```bash
## Deploy for prod env
kubectl apply -f resources/deployments/grouper-ws.yml -n prod

## Deploy for discover env
# kubectl apply -f resources/deployments/grouper-ws.yml -n discover
```
