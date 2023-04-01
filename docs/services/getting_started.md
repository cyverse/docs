!!! Success "Prerequisites"

    * Access to bare metal hardware or :simple-openstack: OpenStack cloud or commercial provider (:simple-amazonaws: AWS, :simple-googlecloud: Google Cloud, :simple-azuredevops: Microsoft Azure)

    * Active :simple-github: GitHub or :simple-gitlab: GitLab account

    * Advanced understanding of :simple-linux: Linux file permissions and Operating Systems

    * Advanced understanding of Virtual Machines and access via `ssh` 

    * Patience

## Installation

[:simple-ansible: Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html){target=_blank}

[:simple-docker: Docker](https://docs.docker.com/engine/install/){target=_blank}

[:simple-kubernetes: Kubernetes Command Line](https://kubernetes.io/docs/tasks/tools/){target=_blank} - `kubectl`

[:simple-terraform: Terraform](https://developer.hashicorp.com/terraform/downloads){target=_blank}

[:simple-visualstudiocode: VS Code Kubernetes Extension](https://code.visualstudio.com/docs/azure/kubernetes){target=_blank} - Optional (recommended) 

## Authentication

[LDAP](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol)

[OAUTH 2.0 protocol](https://www.rfc-editor.org/rfc/rfc6749)

[CILogon](https://www.cilogon.org/home)

## Security Configurations

Experience setting up Firewalls

Experience Living in a [Science DMZ](https://en.wikipedia.org/wiki/Science_DMZ_Network_Architecture)


## API (:material-terrain: Terrain)

[Terrain (API)](services/api_overview.md) is the backbone service which manages the Discovery Environment data science workbench. 

Details about handling API calls in Terrain are presented in the API section.

## :material-hand-extended: Products & Services

[API (Terrain)](api_overview.md) - Terrain API is the main API powering the Discovery Environment workbench

[Authentication](keycloak.md) - managed user authentication uses Keycloak, CI Logon, and OAUTH 2.0 

[BisQue](bisque.md) - large image analyses in the browser

[DataCommons](dc.md) - Community data sharing, DataCite data publishing with DOI

[Data Store](ds.md) - data storage, hosting, & sharing

[DNA Subway](dnasubway.md) - educational software for high school and undergradautes in bioinformatics

[Core Services](services_overview.md) - Core Services for managing CyVerse

[KeyCloak](keycloak.md) - Identity and Access management 

## :simple-kubernetes: Deployments

All of CyVerse primary services and database deployments are containers, controled via fully managed [:simple-kubernetes: Kubernetes](https://kubernetes.io/)

[:simple-kubernetes: Discovery Environment (../de)](../deployments/DiscoveryEnvironment.md) - deploy primary data science workbench site

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

## :octicons-database-24: Databases

[:octicons-database-24: DE](../database/de-db.md) - Discovery Environment Database

[:octicons-database-24: Metadata](../database/metadata-db.md) - the CyVerse Metadata Service

[:octicons-database-24: Keycloak](../database/keycloak-db.md) - database used by Keycloak

[:octicons-database-24: Notifications](../database/notifications-db.md) - database used for notifications

[:octicons-database-24: Unleash](../database/unleash-db.md) - database used for unleash service

[:octicons-database-24: Grouper](../database/grouper-db.md) - database for Grouper service

[:octicons-database-24: QMS](../database/qms-db.md) - database for QMS service

[:octicons-database-24: Portal](../database/portal-db.md) - User Portal database

## :material-map-search-outline: User Guides

[:fontawesome-solid-gears: DevOps](../guides/devops.md) - instructions for setting up a remote DevOps environment to manage a CyVerse deployment

[Discovery Environment](../guides/de.md) - designed for data scientists, students, and researchers who want to use CyVerse for research.

[Data Store](../guides/ds.md) - instructions for using the iRODS Data Store.