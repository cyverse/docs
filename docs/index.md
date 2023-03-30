![](assets/cyverse_logo_2022.svg)

## What is CyVerse?

[CyVerse](https://cyverse.org) is a powerful computational infrastructure and the people who support its operations. 

## Contents

These documents are for managing a self-hosted deployment of CyVerse. They are intended for professional DevOps and IT specialists with experience in Infrastructure as Code

## Development & Operations (DevOps)

The DevOps section includes information about managing CyVerse custom Services, Deployments, and Databases

### Services

* [Core Services](services/services.md) - Core services for managing CyVerse

* [KeyCloak](services/keycloak.md) - Identity and access management 

### Deployments

:simple-kubernetes: [Discovery Environment (DE)](deployments/DiscoveryEnvironment.md) - deploy primary data science workbench site

:simple-kubernetes: [Kubernetes (K8s)](deployments/kubernetes-deploy.md) - deploy the main K8s cluster for running DE applications

:simple-kubernetes: [K8s Resources](deployments/k8s-resources.md) - deploy the various resources in DE managed by K8s

:simple-kubernetes: [K8s NameSpaces](deployments/k8s-namespace.md) - list of namespaces used in DE

:simple-kubernetes: [User Portal](deployments/userportal.md) - deploy the User Portal website via K8s

:simple-kubernetes: [OpenEBS](deployments/openebs.md) - deploy K8s stateful workloads that require container attached storage

:simple-kubernetes: [KeyCloak](deployments/keycloak.md) - deploy K8s KeyCloak configuration

:simple-kubernetes: [Exim4 Mail](deployments/exim4.md) - deploy `exim4` (MTA) running as a smarthost via K8s

:simple-kubernetes: [Redis HA](deployments/redis-ha.md) - installing the Redis Server and Redis Haproxy

:simple-kubernetes: [ElasticSearch](deployments/elasticsearch.md) - deploy stateful set ES cluster

:simple-kubernetes: [RabbitMQ](deployments/RabbitMQ.md) - deploy RabbitMQ services

:simple-kubernetes: [Unleash](deployments/unleash.md) - deploy unleash database

:simple-kubernetes: [Grouper](deployments/grouper.md) - 

:simple-kubernetes: [iRODS CSI Driver](deployments/irods-csi-driver.md) - Container Storage Interface (CSI Driver) for iRODS to K8s pod containers

:simple-kubernetes: [Local Exim](deployments/local-exim.md) - verification of email

:simple-kubernetes: [VICE](deployments/vice.md) - Interactive jobs in DE

:simple-kubernetes: [Jaeger](deployments/jaeger.md) -  open source end-to-end distributed tracing

### Databases

* [DE](database/de-db.md)

* [Metadata](database/metadata-db.md)

* [KeyCloak](database/keycloak-db.md)

* [Notifications](database/notifications-db.md)

* [Unleash](database/unleash-db.md)

* [Grouper](database/grouper-db.md)

* [QMS](database/qms-db.md)

* [Portal](database/portal-db.md)