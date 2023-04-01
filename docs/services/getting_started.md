!!! Success "Prerequisites"

    * Access to the internet and a web-enabled browser

    * Access to bare metal hardware, :simple-openstack: OpenStack cloud, or commercial provider (:simple-amazonaws: AWS, :simple-googlecloud: Google Cloud, :simple-azuredevops: Microsoft Azure)

    * :simple-github: GitHub or :simple-gitlab: GitLab with private repositories for managing sensitive credentials and configurations

    * Advanced understanding of :simple-linux: Linux file permissions and Operating Systems

    * Advanced understanding of Virtual Machines and access via `ssh` 

    * Patience & Perserverance 

## Installation

The [DevOps Guide section](../guides/devops.md) provies a list of required software for managing a CyVerse deployment

## Authentication

CyVerse authentication relies upon [LDAP](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol), [OAUTH 2.0 protocol](https://www.rfc-editor.org/rfc/rfc6749), and [CILogon](https://www.cilogon.org/home)

## :octicons-shield-24: Security Configurations

Experience Living in a [Science DMZ](https://en.wikipedia.org/wiki/Science_DMZ_Network_Architecture) is beneficial for deploying CyVerse on University hardware

## APIs

[:material-terrain: Terrain API](api_overview.md) is the backbone service which manages the Discovery Environment data science workbench. 

Details about Terrain are presented in the [API endpoints section](api/endpoints/endpoints.md)

## :material-hand-extended: Products & Services

[Authentication](keycloak.md) - managed user authentication uses Keycloak, CI Logon, and OAUTH 2.0 

[BisQue](bisque.md) - large image analyses in the browser

[DataCommons](dc.md) - Community data sharing, DataCite data publishing with DOI

[Data Store](ds.md) - data storage, hosting, & sharing

[DNA Subway](dnasubway.md) - educational software for high school and undergradautes in bioinformatics

[Core Services](services_overview.md) - Core Services for managing CyVerse

[KeyCloak](keycloak.md) - Identity and Access management 

## :simple-kubernetes: Deployments

[Deployments](../deployments/deployment_overview.md) are managed via :simple-kubernetes: Kuberentes (K8s)

## :octicons-database-24: Databases

CyVerse uses [:simple-postgresql: PostgreSQL](https://www.postgresql.org/) as its primary Database platform.

Each [Database](../database/main.md) is provisioned separately
