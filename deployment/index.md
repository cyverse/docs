# Deployment

* [Deploying CyVerse from scratch](https://docs.cyverse.org/deployment/from-scratch/) - end-to-end walkthrough of a two-node deployment, phase by phase

The phases below are ordered by dependency, not by preference. iRODS needs
PostgreSQL and RabbitMQ; the Discovery Environment needs all three plus a cluster;
VICE needs the DE. Verify each phase before starting the next.

# Phase 0: planning

* [planning/](https://docs.cyverse.org/deployment/planning) - prerequisites, Ansible, and Docker setup
* [Prerequisites](https://docs.cyverse.org/deployment/planning/prerequisites/) - hardware, skills, tooling, and access
* [Component inventory and sizing](https://docs.cyverse.org/architecture/component-inventory/) - what to provision
* [Network requirements](https://docs.cyverse.org/architecture/network-requirements/) - ports to open before you begin

# Phase 1: foundation

* [01-foundation/](https://docs.cyverse.org/deployment/01-foundation) - the host services everything else depends on
* [HAProxy](https://docs.cyverse.org/deployment/01-foundation/haproxy/) - the public entry point
* [PostgreSQL](https://docs.cyverse.org/deployment/01-foundation/postgresql/) - catalog and service databases
* [RabbitMQ](https://docs.cyverse.org/deployment/01-foundation/rabbitmq/) - the AMQP message bus

# Phase 2: databases

* [02-databases/](https://docs.cyverse.org/deployment/02-databases) - one schema per service
* [Database migrations](https://docs.cyverse.org/deployment/02-databases/migrations/) - the shared migration procedure

# Phase 3: Data Store

* [03-data-store/](https://docs.cyverse.org/deployment/03-data-store) - the iRODS zone
* [iRODS catalog provider](https://docs.cyverse.org/deployment/03-data-store/irods-provider/) - install, policy, and zone initialization
* [iRODS integration for the DE](https://docs.cyverse.org/deployment/03-data-store/de-integration/) - specific queries and service account

# Phase 4: Kubernetes

* [04-kubernetes/](https://docs.cyverse.org/deployment/04-kubernetes) - the cluster and its add-ons
* [Cluster](https://docs.cyverse.org/deployment/04-kubernetes/cluster/) - control plane and workers
* [Cluster resources](https://docs.cyverse.org/deployment/04-kubernetes/resources/) - configuration, secrets, and manifests
* [cert-manager](https://docs.cyverse.org/deployment/04-kubernetes/cert-manager/) - TLS issuance
* [Ingress](https://docs.cyverse.org/deployment/04-kubernetes/ingress/) - Traefik and the legacy ingress-nginx path
* [Storage](https://docs.cyverse.org/deployment/04-kubernetes/storage/) - persistent volumes
* [Harbor](https://docs.cyverse.org/deployment/04-kubernetes/harbor/) - container registry
* [Argo Workflows](https://docs.cyverse.org/deployment/04-kubernetes/argo/) - batch analysis execution

# Phase 5: core services

* [05-core-services/](https://docs.cyverse.org/deployment/05-core-services) - directory, authentication, search, messaging, storage plumbing
* [OpenLDAP](https://docs.cyverse.org/deployment/05-core-services/openldap/) - accounts and groups
* [Keycloak](https://docs.cyverse.org/deployment/05-core-services/keycloak/) - realm, federation, and OAuth clients
* [Grouper](https://docs.cyverse.org/deployment/05-core-services/grouper/) - group management
* [OpenSearch](https://docs.cyverse.org/deployment/05-core-services/opensearch/) - data search index
* [NATS](https://docs.cyverse.org/deployment/05-core-services/nats/) - internal messaging
* [Redis HA](https://docs.cyverse.org/deployment/05-core-services/redis-ha/) - caching and sessions
* [Unleash](https://docs.cyverse.org/deployment/05-core-services/unleash/) - feature flags
* [iRODS CSI driver](https://docs.cyverse.org/deployment/05-core-services/irods-csi-driver/) - Data Store mounts for pods
* [Mail](https://docs.cyverse.org/deployment/05-core-services/mail/) - outbound mail
* [Jaeger](https://docs.cyverse.org/deployment/05-core-services/jaeger/) - distributed tracing

# Phase 6: applications

* [06-applications/](https://docs.cyverse.org/deployment/06-applications) - the user-facing platform
* [Discovery Environment](https://docs.cyverse.org/deployment/06-applications/discovery-environment/) - the DE service set
* [VICE](https://docs.cyverse.org/deployment/06-applications/vice/) - interactive analyses
* [User Portal](https://docs.cyverse.org/deployment/06-applications/user-portal/) - account management

# Phase 7: post-install

* [07-post-install/](https://docs.cyverse.org/deployment/07-post-install) - making the deployment usable and confirming it works
* [Bootstrap](https://docs.cyverse.org/deployment/07-post-install/bootstrap/) - first administrator, VICE operator, starting apps
* [Verification](https://docs.cyverse.org/deployment/07-post-install/verification/) - end-to-end checks
* [Troubleshooting](https://docs.cyverse.org/deployment/07-post-install/troubleshooting/) - fixes for first-deployment problems

# After deployment

* [operations/](https://docs.cyverse.org/operations) - day-to-day administration
* [FAQ](https://docs.cyverse.org/operations/faq/) - common questions and recipes
