---
type: Deployment Procedure
title: "OpenSearch"
description: "Deploying the search cluster that indexes Data Store contents and metadata for the DE."
tags: [deployment, core-services, opensearch, search]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: opensearch-docs
    resource: https://opensearch.org/docs/latest/
    title: OpenSearch documentation
    author: team:opensearch
---

# Role in the deployment

The search cluster indexes Data Store paths, names, and AVU metadata so the DE
can answer data searches without walking the catalog. It is populated by
`infosquito2`, which consumes iRODS data events from
[RabbitMQ](../01-foundation/rabbitmq.md) and can also be asked for a full
reindex.

OpenSearch is what new deployments use. Older deployments run Elasticsearch; that
path is kept in [Elasticsearch (legacy)](./elasticsearch.md) for the clusters
still on it.

# Install

```bash
ansible-playbook -i /path/to/inventory --tags opensearch kubernetes.yml
```

Requires a working storage class — see [storage](../04-kubernetes/storage.md).
Index data is regenerable, but a full reindex of a large zone is measured in
hours, so give it durable volumes rather than node-local scratch.

# Verify

```bash
kubectl -n <NAMESPACE> get pods -l app=opensearch
kubectl -n <NAMESPACE> exec -it <pod> -- curl -s localhost:9200/_cluster/health?pretty
```

On a small cluster expect `yellow` rather than `green` when replicas are
configured but there are not enough nodes to place them. That is a capacity
statement, not a failure; either add nodes or lower the replica count.

# Populating the index

1. Confirm `infosquito2` is running and consuming from the DE exchange.
2. Trigger a full reindex through the message bus:
   [reindex search](../01-foundation/rabbitmq.md#operations-reindex-search).
3. Restart `infosquito2` and the `search` service so they pick up the request:

   ```bash
   kubectl rollout restart deployment infosquito2 search -n <NAMESPACE>
   ```

A reindex reads the entire catalog and loads both PostgreSQL and the search
cluster. Run it deliberately, not as a first troubleshooting step.

# Related

* [Elasticsearch (legacy)](./elasticsearch.md)
* [RabbitMQ](../01-foundation/rabbitmq.md)
* [iRODS integration for the DE](../03-data-store/de-integration.md)
