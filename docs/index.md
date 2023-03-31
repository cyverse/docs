![](assets/cyverse_logo_2022.svg)

This [DevOps Documentation](services/system_overview.md) site is *primarily* intended for professional DevOps and IT specialists with experience in Infrastructure as Code (IaC), Kubernetes (K8s), OpenStack Cloud, and Networking who are interested in deploying their own CyVerse infrastructure or to leverage components of the primary CyVerse deployment managed by University of Arizona, Texas Advanced Computing Center (TACC), and Cold Spring Harbor Labs (CSHL). 

## :fontawesome-solid-folder-tree: Contents

The [User Guide documentation](guides/devops.md) is intended for domain scientists and data scientists interested in using the platform. 

[DevOps Overview](#development--operations-devops)

[System Overview](services/system_overview.md) - description of physical resources used to manage and operate CyVerse

[API (Terrain) Overview](services/api_overview.md) - documentation for API endpoints 

[Services Overview](#services) - list of services within CyVerse

[Deployments Overview](#simple-kubernetes-deployments) - instructions for deploying APIs, databases, and services

[Databases Overview](#octicons-database-24-databases) - list of databases used within CyVerse

[User Guide Overview](#user-guides)

??? Question "What is CyVerse?"

    CyVerse is a powerful computational infrastructure and the people who support its operations. 
    
    CyVerse has been fully funded by the United States national Science Foundation (NSF) since 2008

    [Public Website](https://cyverse.org)

    [![NSF-0735191](https://img.shields.io/badge/NSF-0735191-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=0735191) [![NSF-1265383](https://img.shields.io/badge/NSF-1265383-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1265383) [![NSF-1743442](https://img.shields.io/badge/NSF-1743442-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1743442)


!!! Info ":fontawesome-brands-creative-commons-by: SOFTWARE LICENSE"

    Copyright (c) 2010-2023, The Arizona Board of Regents on behalf of The University of Arizona

    All rights reserved.

    Developed by: CyVerse as a collaboration between participants at BIO5 at The University of Arizona (the primary hosting institution), Cold Spring Harbor Laboratory, The University of Texas at Austin, and individual contributors. Find out more at http://www.cyverse.org/.

    Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

    * Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
    * Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
    * Neither the name of CyVerse, BIO5, The University of Arizona, Cold Spring Harbor Laboratory, The University of Texas at Austin, nor the names of other contributors may be used to endorse or promote products derived from this software without specific prior written permission.

    THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

### :fontawesome-solid-gears: Development & Operations (DevOps)

Details about managing Services, Deployments, and Databases as IaC:

### API (:material-terrain: Terrain)

[Terrain (API)](services/api_overview.md) is the backbone service which manages the Discovery Environment data science workbench. 

Details about handling API calls in Terrain are presented in the API section.

### :material-hand-extended: Services

User facing services include:

* [API (Terrain)](services/api_overview.md) - Terrain API is the main API powering the Discovery Environment workbench

* [Authentication](services/keycloak.md) - managed user authentication using OAUTH and KeyCloak

* [BisQue](services/bisque.md) - large scale image analyses

* [DataCommons](services/dc.md) - Community data sharing, DataCite data publishing with DOI

* [Data Store](services/ds/md) - data storage, hosting, & sharing

* [DNA Subway](services/dnasubway.md) - educational software for high school and undergradautes in bioinformatics

* [Core Services](services/services_overview.md) - Core Services for managing CyVerse

* [KeyCloak](services/keycloak.md) - Identity and Access management 


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

### :material-map-search-outline: User Guides

[:fontawesome-solid-gears: DevOps](guides/devops.md) - instructions for setting up a remote DevOps environment to manage a CyVerse deployment

[Discovery Environment](guides/de.md) - designed for data scientists, students, and researchers who want to use CyVerse for research.

[Data Store](guides/ds.md) - instructions for using the iRODS Data Store.