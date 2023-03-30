
[de]: ../assets/de/deIcon.svg
[data]: ../assets/de/dataIcon.svg
[cacao]: ../assets/de/cacao-04.png
[ball]: ../assets/de/cyverse_ball_2022.png

## :material-api: Application Programming Interfaces (APIs)

[:simple-swagger: :material-terrain: Terrain API](https://de.cyverse.org/terrain/docs/index.html){target=_blank} - runs on Swagger API

[:simple-openapiinitiative: :simple-gitlab: CACAO API](https://gitlab.com/cyverse/cacao/-/blob/master/docs/openapi/openapi.yaml){target=_blank} - runs on OpenAPI

[:simple-openapiinitiative: :simple-gitlab: Data Watch API](https://gitlab.com/cyverse/datawatch/-/blob/master/docs/openapi/datawatch-openapi.yaml){target=_blank} - runs on OpenAPI

## :octicons-cloud-24: Cloud Services

[![][cacao]{width=25}](https://cyverse.org/cacao){target=_blank} [:chocolate_bar: Continous Automation / Continuous Analysis & Orchestration (CACAO)](https://cyverse.org/cacao){target=_blank} - Infrastructure as Code for multi-cloud deployments

* [:simple-terraform: :simple-gitlab: CACAO Terraform Templates](https://gitlab.com/cyverse/cacao-tf-os-ops/){target=_blank}

[![][cacao]{width=25}](https://gitlab.com/cyverse/datawatch){target=_blank} [:octicons-stopwatch-24: DataWatch](https://gitlab.com/cyverse/datawatch){target=_blank} - a notification system for reporting data events

* [:simple-gitlab: DataWatch](https://gitlab.com/cyverse/datawatch){target=_blank}

## :material-server: Compute Resources

:simple-kubernetes: Kubernetes Clusters

* the Discovery Environment data science workbench leverages on-premises hardware located at University of Arizona in the UITS colocation space at the high performance computing center. 

* leveraging [National Research Platform](https://nationalresearchplatform.org/){target=_blank} federated K8s resources are in development for the Discovery Environment (DE).

:material-server: High Throughput Computing Environments:

![htcondor](../assets/HTCondor_red_blk.svg){width=250}

DE cluster uses [HTCondor](https://htcondor.org/){target=_blank}

![](../assets/OSG_Logo_W_Text.svg){width=250}

Federation to the [OpenScienceGrid](https://opensciencegrid.org){target=_blank} can be accomplished in the DE

:simple-openstack: OpenStack Cloud 

* CyVerse maintains its own OpenStack Cloud (formerly "Atmosphere") for internal use and development of CACAO.

* Jetstream2 is primarily operated at Indiana University, but test clusters are shared across other universities in the US

![js2](../assets/js2.png)

:material-server: High Performance Computing Environments

* University of Arizona resources are colocated with the CyVerse data store and compute clusters

* CyVerse is partnered with [Texas Advanced Computing Center (TACC)](https://www.tacc.utexas.edu/){target=_blank} where its data store is replicated nightly

    * [ACCESS-CI allocation request]()

    * [TACC Allocation request]()
    

## :octicons-database-24: Data Storage

CyVerse manages over 6 PB data via [iRODS (integrated Rule Oriented Data System)](https://irods.org){target=_blank} with two unique zones:

* `iplant` zone is the original project name, and is retained in production for all user accounts.

* `cyverse` zone is used in the CyVerse QA Environment

The iRODS data store is mirrored between University of Arizona and Texas Advanced Computing Center (TACC) nightly.

Resource Servers coordinate resources and contain physical storage devices. An iCAT server stores metadata in the form of “triples” to its relational database. 

## :material-web: Web Portals

[![][ball]{width=25}](https://user.cyverse.org/){target=_blank} [:material-web: User Portal](https://user.cyverse.org){target=_blank} - a User Portal for creating and managing accounts, requesting and granting access to platforms, and a user management space for individuals and groups and workshops. 

[![][de]{width=25}](https://de.cyverse.org){target=_blank} [:material-web: Discovery Environment](https://de.cyverse.org){target=_blank}  - Custom interactive web based data science workbench

[:material-shield-key: :material-web: KeyCloak](https://kc.cyverse.org){target=_blank} - federated OAUTH to CyVerse resources, including Google, GitHub, ORCID,& CILogon

[![][data]{width=25}](https://data.cyverse.org){target=_blank} [:material-web: WebDav](https://data.cyverse.org/){target=_blank} - `https://` endpoint for the iRODS data store

[![][data]{width=25}](){target=_blank} [:material-web: SFTP](/){target=_blank} - built using SFTPGo

[![][data]{width=25}](https://datacommons.cyverse.org){target=_blank} [:material-web: DataCommons](https://datacommons.cyverse.org/){target=_blank} - website hosting of published datasets in the CyVerse data store. The DataCommons presents any metadata which have been added by the owners to directories (folders) or files.

## :fontawesome-solid-staff-snake: Monitoring Services

[:material-web: Health Status](https://status.cyverse.org/){target=_blank} - system status monitor

[:material-crosshairs-gps: perfSONAR web toolkit](http://206.207.252.45/toolkit/){target=_blank} - network measurement toolkit
