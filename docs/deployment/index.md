# Deployment

* [Deploying CyVerse from scratch](from-scratch.md) - end-to-end walkthrough of a two-node deployment, phase by phase

The phases below are ordered by dependency, not by preference. iRODS needs
PostgreSQL and RabbitMQ; the Discovery Environment needs all three plus a cluster;
VICE needs the DE. Verify each phase before starting the next.

# Phase 0: planning

* [planning/](planning/) - prerequisites, Ansible, and Docker setup
* [Prerequisites](planning/prerequisites.md) - hardware, skills, tooling, and access
* [Component inventory and sizing](../architecture/component-inventory.md) - what to provision
* [Network requirements](../architecture/network-requirements.md) - ports to open before you begin

# Phase 1: foundation

* [01-foundation/](01-foundation/) - the host services everything else depends on
* [HAProxy](01-foundation/haproxy.md) - the public entry point
* [PostgreSQL](01-foundation/postgresql.md) - catalog and service databases
* [RabbitMQ](01-foundation/rabbitmq.md) - the AMQP message bus

# Phase 2: databases

* [02-databases/](02-databases/) - one schema per service
* [Database migrations](02-databases/migrations.md) - the shared migration procedure

# Phase 3: Data Store

* [03-data-store/](03-data-store/) - the iRODS zone
* [iRODS catalog provider](03-data-store/irods-provider.md) - install, policy, and zone initialization
* [iRODS integration for the DE](03-data-store/de-integration.md) - specific queries and service account

# Phase 4: Kubernetes

* [04-kubernetes/](04-kubernetes/) - the cluster and its add-ons
* [Cluster](04-kubernetes/cluster.md) - control plane and workers
* [Cluster resources](04-kubernetes/resources.md) - configuration, secrets, and manifests
* [cert-manager](04-kubernetes/cert-manager.md) - TLS issuance
* [Ingress](04-kubernetes/ingress.md) - Traefik and the legacy ingress-nginx path
* [Storage](04-kubernetes/storage.md) - persistent volumes
* [Harbor](04-kubernetes/harbor.md) - container registry
* [Argo Workflows](04-kubernetes/argo.md) - batch analysis execution

# Phase 5: core services

* [05-core-services/](05-core-services/) - directory, authentication, search, messaging, storage plumbing
* [OpenLDAP](05-core-services/openldap.md) - accounts and groups
* [Keycloak](05-core-services/keycloak.md) - realm, federation, and OAuth clients
* [Grouper](05-core-services/grouper.md) - group management
* [OpenSearch](05-core-services/opensearch.md) - data search index
* [NATS](05-core-services/nats.md) - internal messaging
* [Redis HA](05-core-services/redis-ha.md) - caching and sessions
* [Unleash](05-core-services/unleash.md) - feature flags
* [iRODS CSI driver](05-core-services/irods-csi-driver.md) - Data Store mounts for pods
* [Mail](05-core-services/mail.md) - outbound mail
* [Jaeger](05-core-services/jaeger.md) - distributed tracing

# Phase 6: applications

* [06-applications/](06-applications/) - the user-facing platform
* [Discovery Environment](06-applications/discovery-environment.md) - the DE service set
* [VICE](06-applications/vice.md) - interactive analyses
* [User Portal](06-applications/user-portal.md) - account management

# Phase 7: post-install

* [07-post-install/](07-post-install/) - making the deployment usable and confirming it works
* [Bootstrap](07-post-install/bootstrap.md) - first administrator, VICE operator, starting apps
* [Verification](07-post-install/verification.md) - end-to-end checks
* [Troubleshooting](07-post-install/troubleshooting.md) - fixes for first-deployment problems

# After deployment

* [operations/](../operations/) - day-to-day administration
* [FAQ](../operations/faq.md) - common questions and recipes
