---
type: Deployment Procedure
title: "NATS"
description: "Deploying the NATS messaging layer used for internal service-to-service communication."
tags: [deployment, core-services, nats, messaging]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: nats-docs
    resource: https://docs.nats.io/
    title: NATS documentation
    author: team:nats-io
---

# Role in the deployment

NATS carries in-cluster messages between DE services. It sits alongside
[RabbitMQ](../01-foundation/rabbitmq.md) rather than replacing it: RabbitMQ is
the AMQP bus that iRODS publishes data events to from outside the cluster, while
NATS is the lower-latency internal fabric newer DE services use.

# Install

NATS is installed together with the global configuration, ingress, and networking
resources, because those services expect it to be there when they start:

```bash
ansible-playbook -i /path/to/inventory \
  --tags=configure-services,ingress,networking,nats kubernetes.yml
```

# Reinstalling

Helm keeps a release record even after the resources are gone, so a reinstall
into the same namespace can fail with an "already exists" error. Remove the
release first:

```bash
helm -n prod uninstall nats
```

Then re-run the tag above.

# Verify

```bash
kubectl -n prod get pods -l app.kubernetes.io/name=nats
kubectl -n prod logs -l app.kubernetes.io/name=nats --tail=50
```

# Related

* [Cluster resources](../04-kubernetes/resources.md)
* [RabbitMQ](../01-foundation/rabbitmq.md)
