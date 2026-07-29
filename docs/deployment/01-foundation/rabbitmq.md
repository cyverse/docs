---
type: Deployment Procedure
title: "RabbitMQ"
description: "Installing the AMQP broker, creating the Data Store vhost and accounts, and reindexing search from the message bus."
tags: [deployment, foundation, rabbitmq, amqp]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: rabbitmq-docs
    resource: https://www.rabbitmq.com/docs
    title: RabbitMQ documentation
    author: team:rabbitmq
---

# Role in the deployment

RabbitMQ is the message bus between iRODS and the DE. iRODS publishes data
events to it; DE services consume them to keep search indexes and notifications
current. It is also how a full search reindex is triggered.

It is a host service, not a Kubernetes workload — it is installed on the node in
phase 1, before anything that depends on it. Sizing is modest: 1 core, 2 GB
memory, 20 GB storage.

# Install and configure

Run on the foundation node (`core-1`):

1. Install `rabbitmq-server` with the OS package manager.
2. Enable the management plugin and restart the service:

   ```bash
   rabbitmq-plugins enable rabbitmq_management
   systemctl restart rabbitmq-server
   ```

3. Create an administrator account with a generated password, grant it full
   configure, write, and read permissions, and tag it `administrator`.
4. **Delete the default `guest` account.** It ships with well-known credentials.
5. Create the vhost `/data-store` and grant the administrator full configure,
   write, and read permissions on it.
6. Create a separate account for iRODS with its own generated password and full
   configure, write, and read permissions on `/data-store` — iRODS should not
   authenticate as the administrator.
7. Create a `topic` exchange named `irods` on `/data-store`.

The iRODS account's credentials go into `IRODS_AMQP_URI` in
`/etc/irods/server_config.json`; see
[iRODS provider](../03-data-store/irods-provider.md).

The DE's own vhost, exchange, and accounts are created later, in phase 6, by
the deployment repository's `rabbitmq_configure.yml` playbook:

```bash
ansible-playbook -i /path/to/inventory rabbitmq_configure.yml
```

The existing Data Store playbooks (`playbooks/amqp.yml`,
`playbooks/amqp_exchange.yml` in the
[Data Store collection](https://github.com/cyverse/ds-collection)) automate
steps 1 through 7 and are worth reusing rather than doing this by hand.

# Ports

| Port | Purpose | Scope |
|------|---------|-------|
| 5672 | AMQP | Analysis nodes and in-cluster services |
| 15672 | Management UI and `rabbitmqadmin` download | Administrators only |

Do not expose `15672` publicly. See
[network requirements](../../architecture/network-requirements.md).

# Operations: reindex search

A full reindex is requested by publishing an empty message with the routing key
`index.all` to the DE exchange, then restarting the indexer.

## One-time: get `rabbitmqadmin`

`rabbitmqadmin` is served by the management plugin, so fetch it from the broker
host itself:

```bash
ssh <RABBITMQ_HOST>
mkdir -p ~/adm && cd ~/adm
wget http://localhost:15672/cli/rabbitmqadmin
chmod +x rabbitmqadmin
```

## Trigger the reindex

Replace `<VHOST>`, `<USER>`, and `<NAMESPACE>` with the values for the
environment you are working in — the DE vhost and user come from the deployment
group variables, and the namespace is where the DE services run (`prod` in a
standard deployment).

```bash
# check the broker is healthy
systemctl status rabbitmq-server.service -l

# read the password into the environment instead of putting it in shell history
read -rs PASSWORD && export PASSWORD

# confirm you are pointed at the right vhost
./rabbitmqadmin -V <VHOST> list exchanges -u <USER> -p "$PASSWORD"

# request a full reindex
./rabbitmqadmin publish -V <VHOST> -u <USER> -p "$PASSWORD" \
    exchange=de routing_key=index.all payload=""

# restart the indexer so it picks the request up
kubectl rollout restart deployment infosquito2 -n <NAMESPACE>
```

If `infosquito2` is not deployed at all, deploy it rather than restarting it —
see [cluster resources](../04-kubernetes/resources.md).

A reindex reads the entire catalog. On a large zone it takes hours and adds load
to both PostgreSQL and the search cluster, so run it deliberately.

# Related

* [OpenSearch](../05-core-services/opensearch.md)
* [Elasticsearch (legacy)](../05-core-services/elasticsearch.md)
* [iRODS provider](../03-data-store/irods-provider.md)
* [Deploying from scratch](../from-scratch.md#13-rabbitmq)
