!!! Success "Prerequisites"

    * Access to the internet and a web-enabled browser

    * Access to bare metal hardware, :simple-openstack: OpenStack cloud, or commercial provider (:simple-amazonaws: AWS, :simple-googlecloud: Google Cloud, :simple-azuredevops: Microsoft Azure) to deploy the Infrastructure as Code (IaC)

    * :simple-github: GitHub or :simple-gitlab: GitLab with private repositories for managing sensitive credentials and configurations

    * Advanced understanding of :simple-linux: Linux file permissions and Operating Systems

    * Advanced understanding of Virtual Machines and access via `ssh` 

    * Patience & Perserverance 

## Installation

The DevOps section requires the following software:

[:simple-ansible: Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html){target=_blank}

[:simple-docker: Docker](https://docs.docker.com/engine/install/){target=_blank}

[:simple-kubernetes: Kubernetes (K8s)](https://kubernetes.io/docs/tasks/tools/){target=_blank} - `kubectl` for managing K8s clusters

[:simple-terraform: Terraform](https://developer.hashicorp.com/terraform/downloads){target=_blank}

[:simple-visualstudiocode: VS Code Kubernetes Extension](https://code.visualstudio.com/docs/azure/kubernetes){target=_blank} - optional (recommended) 

## Authentication

CyVerse authentication relies upon [LDAP](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol), [OAUTH 2.0 protocol](https://www.rfc-editor.org/rfc/rfc6749), and [CILogon](https://www.cilogon.org/home)

## :octicons-shield-24: Security Configurations

Experience Living in a [Science DMZ](https://en.wikipedia.org/wiki/Science_DMZ_Network_Architecture) is beneficial for deploying CyVerse on University hardware

## APIs

[:material-terrain: Terrain API](services/api_overview.md) is the backbone service which manages the Discovery Environment data science workbench. 

Details about Terrain are presented in the [API endpoints section](services/api/endpoints/endpoints.md)

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
