![](assets/cyverse_logo_2022.svg)

## Contents

These documents are for managing a fully self-hosted deployment of CyVerse. 

These pages are intended for primarily for professional DevOps and IT specialists with experience in Infrastructure as Code (IaC).

Our [User Guides]() are designed for data scientists, students, and researchers who want to use CyVerse for research.

??? Question "What is CyVerse?"

    CyVerse is a powerful computational infrastructure and the people who support its operations. 
    
    CyVerse has been fully funded by the United States national Science Foundation (NSF) since 2008

    [Public Website](https://cyverse.org)

    [![NSF-0735191](https://img.shields.io/badge/NSF-0735191-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=0735191) [![NSF-1265383](https://img.shields.io/badge/NSF-1265383-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1265383) [![NSF-1743442](https://img.shields.io/badge/NSF-1743442-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1743442)


## Development & Operations (DevOps)

Details about managing Services, Deployments, and Databases as IaC:

### Services

User facing services include:

* [API (Terrain)](services/api_overview.md) - Terrain API is the main API powering the Discovery Environment workbench

* [Authentication](services/keycloak.md) - managed user authentication using OAUTH and KeyCloak

* [BisQue](services/bisque.md) - large scale image analyses

* [DataCommons](services/dc.md) - Community data sharing, DataCite data publishing with DOI

* [Data Store](services/ds/md) - storage, hosting, sharing

* [DNA Subway](services/dnasubway.md) - educational software for high school and undergradautes in bioinformatics

* [Core Services](services/services.md) - Core services for managing CyVerse

* [KeyCloak](services/keycloak.md) - Identity and access management 


### :simple-kubernetes: Deployments

All of CyVerse primary services and database deployments are containers, controled via fully managed [:simple-kubernetes: Kubernetes](https://kubernetes.io/)

[:simple-kubernetes: Discovery Environment (DE)](deployments/DiscoveryEnvironment.md) - deploy primary data science workbench site

[:simple-kubernetes: Kubernetes (K8s)](deployments/kubernetes-deploy.md) - deploy the main K8s cluster for running DE applications

[:simple-kubernetes: K8s Resources](deployments/k8s-resources.md) - deploy the various resources in DE managed by K8s

[:simple-kubernetes: K8s NameSpaces](deployments/k8s-namespace.md) - list of namespaces used in DE

[:simple-kubernetes: User Portal](deployments/userportal.md) - deploy the User Portal website via K8s

[:simple-kubernetes: OpenEBS](deployments/openebs.md) - deploy K8s stateful workloads that require container attached storage

[:simple-kubernetes: KeyCloak](deployments/keycloak.md) - deploy K8s KeyCloak configuration

[:simple-kubernetes: Exim4 Mail](deployments/exim4.md) - deploy `exim4` (MTA) running as a smarthost via K8s

[:simple-kubernetes: Redis HA](deployments/redis-ha.md) - installing the Redis Server and Redis Haproxy

[:simple-kubernetes: ElasticSearch](deployments/elasticsearch.md) - deploy stateful set ES cluster

[:simple-kubernetes: RabbitMQ](deployments/RabbitMQ.md) - deploy RabbitMQ services

[:simple-kubernetes: Unleash](deployments/unleash.md) - deploy the Unleash database

[:simple-kubernetes: Grouper](deployments/grouper.md) - Internet2 Grouper Service

[:simple-kubernetes: iRODS CSI Driver](deployments/irods-csi-driver.md) - K8s Container Storage Interface (CSI Driver) for iRODS 

[:simple-kubernetes: Local Exim](deployments/local-exim.md) - verification of email using Exim

[:simple-kubernetes: VICE](deployments/vice.md) - Manage K8s interactive jobs in DE

[:simple-kubernetes: Jaeger](deployments/jaeger.md) - open-source end-to-end distributed tracing

### :octicons-database-24: Databases

[:octicons-database-24: DE](database/de-db.md) - Discovery Environment Database

[:octicons-database-24: Metadata](database/metadata-db.md) - the CyVerse Metadata Service

[:octicons-database-24: Keycloak](database/keycloak-db.md) - database used by Keycloak

[:octicons-database-24: Notifications](database/notifications-db.md) - database used for notifications

[:octicons-database-24: Unleash](database/unleash-db.md) - database used for unleash service

[:octicons-database-24: Grouper](database/grouper-db.md) - database for Grouper service

[:octicons-database-24: QMS](database/qms-db.md) - database for QMS service

[:octicons-database-24: Portal](database/portal-db.md) - User Portal database