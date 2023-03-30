# Unleash

!!! success "Prerequisites"

    * Make sure `unleash` database is setup, see also [unleash-db](../database/unleash-db.md)

## Deploy

```bash
## Deploy for prod env
kubectl apply -f resources/deployments/unleash.yml -n prod

## Deploy for discover env
# kubectl apply -f resources/deployments/unleash.yml -n discover
```
