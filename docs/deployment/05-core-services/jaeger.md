---
type: Deployment Procedure
title: "Jaeger"
description: "Deploying Jaeger for end-to-end distributed tracing of DE services."
tags: [deployment, core-services, jaeger, observability, tracing]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

# Role in the deployment

[Jaeger](https://www.jaegertracing.io/) collects distributed traces from DE
services, which is how a slow request is attributed to a specific service rather
than to "the DE". Services send spans to the collector endpoint configured in the
`Jaeger` section of the deployment group variables.

Jaeger is optional. Nothing user-facing depends on it, and it can be added to a
running deployment later.

# Prerequisites

* A search cluster to store spans in — [OpenSearch](./opensearch.md), or
  [Elasticsearch](./elasticsearch.md) in older deployments.
* The manifests from [cluster resources](../04-kubernetes/resources.md).

# Deploy

```bash
kubectl create ns jaeger

kubectl apply -f resources/addons/jaeger/rollover-cron.yaml -n jaeger
kubectl apply -f resources/addons/jaeger/query.yaml         -n jaeger
kubectl apply -f resources/addons/jaeger/collector.yaml     -n jaeger
```

Each of those three manifests names the search cluster it talks to. If your
search cluster is not in the `prod` namespace, update the endpoint in all three
before applying:

```diff
- "http://elasticsearch.prod:9200"
+ "http://<SEARCH_SERVICE>.<NAMESPACE>:9200"
```

The rollover cron job is what keeps span indices from growing without bound.
Deploy it, not just the collector and query components — a Jaeger install without
rollover fills its storage and then takes the search cluster down with it.

# Verify

```bash
kubectl -n jaeger get pods
kubectl -n jaeger logs deploy/jaeger-collector --tail=50
```

# Related

* [OpenSearch](./opensearch.md)
* [Cluster resources](../04-kubernetes/resources.md)
