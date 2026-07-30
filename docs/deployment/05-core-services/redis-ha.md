---
type: Deployment Procedure
title: "Redis HA"
description: "Deploying the Redis server, Sentinel, and Redis HAProxy used for caching and sessions."
tags: [deployment, core-services, redis, cache, sessions]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

# Role in the deployment

Redis backs DE caching and session state. It is deployed as three replicas with
Sentinel for failover, plus a Redis HAProxy that gives clients one address to
connect to instead of tracking which replica is currently primary. All of it runs
in the same namespace as the DE services.

The DE reads its Redis configuration from the `Redis` section of the deployment
group variables: host, port, HA service name, password, and database number.

# Values

Create a values file that overrides the persistent volume and sets the
credentials. Keep it in your **private** inventory — it contains two secrets.

```yaml
## replicas for each component
replicas: 3

persistentVolume:
  enabled: true
  ## storage class for redis-ha data
  ## use your cluster's replicated class; see the storage document
  storageClass: <STORAGE_CLASS>

## Sentinel
sentinel:
  auth: true
  authkey: <GENERATED_SECRET>
  password: <GENERATED_SECRET>

## Redis
auth: true
authkey: <GENERATED_SECRET>
redisPassword: <GENERATED_SECRET>
```

!!! note "Storage class"

    Older deployments set `storageClass: openebs-hostpath`, which is node-local:
    a replica pinned to a lost node loses its data. Use your Longhorn class in a
    new deployment — see [storage](../04-kubernetes/storage.md).

# Deploy Redis and Sentinel

```bash
helm repo add dandydev https://dandydeveloper.github.io/charts
helm repo update

helm upgrade --install redis-ha dandydev/redis-ha \
  --namespace <NAMESPACE> --values values.yaml
```

# Deploy Redis HAProxy

The HAProxy in front of Redis reads the configuration and secrets loaded by
[cluster resources](../04-kubernetes/resources.md), so load those first, then:

```bash
kubectl apply -f resources/deployments/redis-haproxy.yml -n <NAMESPACE>
```

# Verify

```bash
kubectl -n <NAMESPACE> get pods -l app=redis-ha
kubectl -n <NAMESPACE> exec -it redis-ha-server-0 -c redis -- \
    redis-cli -a "$REDIS_PASSWORD" info replication
```

Expect one `master` and two `slave` roles. Three replicas all reporting `master`
means Sentinel is not forming a quorum — check that the Sentinel auth key matches
across replicas.

# Related

* [Storage](../04-kubernetes/storage.md)
* [Cluster resources](../04-kubernetes/resources.md)
