# Deployment Overview

All deployments of CyVerse Products and Services are managed through Kubernetes.

Each service is maintained in its own GitHub Repository in the core [:simple-github: CyVerse Organization](https://github.com/cyverse){target=_blank} or [:simple-github: CyVerse Discovery Environment Organization](https://github.com/cyverse-de){target=_blank}

## :simple-kubernetes: Deployments

All of CyVerse primary services and database deployments are containers, controled via fully managed [:simple-kubernetes: Kubernetes](https://kubernetes.io/)

[:simple-kubernetes: Discovery Environment](../deployments/DiscoveryEnvironment.md) - deploy primary data science workbench site

[:simple-kubernetes: Kubernetes (K8s)](../deployments/kubernetes-deploy.md) - deploy the main K8s cluster for running DE applications

[:simple-kubernetes: K8s Resources](../deployments/k8s-resources.md) - deploy the various resources in DE managed by K8s

[:simple-kubernetes: K8s NameSpaces](../deployments/k8s-namespace.md) - list of namespaces used in DE

[:simple-kubernetes: User Portal](../deployments/userportal.md) - deploy the User Portal website via K8s

[:simple-kubernetes: OpenEBS](../deployments/openebs.md) - deploy K8s stateful workloads that require container attached storage

[:simple-kubernetes: KeyCloak](../deployments/keycloak.md) - deploy K8s KeyCloak configuration

[:simple-kubernetes: Exim4 Mail](../deployments/exim4.md) - deploy `exim4` (MTA) running as a smarthost via K8s

[:simple-kubernetes: Redis HA](../deployments/redis-ha.md) - installing the Redis Server and Redis Haproxy

[:simple-kubernetes: ElasticSearch](../deployments/elasticsearch.md) - deploy stateful set ES cluster

[:simple-kubernetes: RabbitMQ](../deployments/RabbitMQ.md) - deploy RabbitMQ services

[:simple-kubernetes: Unleash](../deployments/unleash.md) - deploy the Unleash database

[:simple-kubernetes: Grouper](../deployments/grouper.md) - Internet2 Grouper Service

[:simple-kubernetes: iRODS CSI Driver](../deployments/irods-csi-driver.md) - K8s Container Storage Interface (CSI Driver) for iRODS 

[:simple-kubernetes: Local Exim](../deployments/local-exim.md) - verification of email using Exim

[:simple-kubernetes: VICE](../deployments/vice.md) - Manage K8s interactive jobs in DE

[:simple-kubernetes: Jaeger](../deployments/jaeger.md) - open-source end-to-end distributed tracing
