
[de]: ../assets/de/deIcon.svg
[data]: ../assets/de/dataIcon.svg
[cacao]: ../assets/de/cacao-04.png
[ball]: ../assets/de/cyverse_ball_2022.png

## :material-api: Application Programming Interfaces (APIs)

CyVerse manages public-facing APIs which is leveraged in "[Powered-by-CyVerse](https://cyverse.org/powered-by-cyverse)" projects. 

All APIs use [:simple-openapiinitiative: OpenAPI](https://www.openapis.org/)

[:material-terrain: Terrain API](https://de.cyverse.org/terrain/docs/index.html){target=_blank} - main API for Discovery Environment Swagger interface 

* [:simple-jupyter: Terrain API Jupyter Notebooks](https://github.com/cyverse/terrain-notebook)

* [:simple-swagger: https://de.cyverse.org/terrain/swagger.json](https://de.cyverse.org/terrain/swagger.json)

[CACAO API](https://gitlab.com/cyverse/cacao/-/blob/master/docs/openapi/openapi.yaml){target=_blank} - Infrastructure as Code API for cloud automation with OpenAPI

[Data Watch API](https://gitlab.com/cyverse/datawatch/-/blob/master/docs/openapi/datawatch-openapi.yaml){target=_blank} - event based triggers for workflows with OpenAPI

## :octicons-cloud-24: Cloud Services

[![][cacao]{width=25}](https://cyverse.org/cacao){target=_blank} [Continous Automation / Continuous Analysis & Orchestration (CACAO)](https://cyverse.org/cacao){target=_blank} - Infrastructure as Code for multi-cloud deployments

* [:simple-terraform: CACAO Terraform Templates](https://gitlab.com/cyverse/cacao-tf-os-ops/){target=_blank}

* [:octicons-stopwatch-24: DataWatch](https://gitlab.com/cyverse/datawatch){target=_blank} - a notification system for reporting data events

## :material-server: Compute Resources

The DE runs on-premises hardware located at University of Arizona in the UITS colocation space at the high performance computing center. 

### :simple-kubernetes: Kubernetes Clusters

* The DE runs on locally managed K8s clusters.

* [National Research Platform](https://nationalresearchplatform.org/){target=_blank} federated K8s resources are in development for the DE.

### :simple-openstack: OpenStack Cloud 

* CyVerse maintains its own OpenStack Cloud (formerly "Atmosphere") for internal use and development of CACAO.

* Jetstream2 is primarily operated at Indiana University, but test clusters are shared across other universities in the US

![js2](../assets/js2.png)

### :material-server: High Throughput Computing Environments:

![htcondor](../assets/HTCondor_red_blk.svg){width=250}

DE uses [HTCondor](https://htcondor.org/){target=_blank} for `executable` jobs on CyVerse resources and `osg` jobs on the [OpenScienceGrid](https://opensciencegrid.org){target=_blank}

![](../assets/OSG_Logo_W_Text.svg){width=250}

Federation to the [OpenScienceGrid](https://opensciencegrid.org){target=_blank} can be accomplished in the DE

### :material-server: High Performance Computing Environments

[University of Arizona](https://it.arizona.edu/){target=_blank} resources are colocated with the CyVerse data store and compute clusters

CyVerse is partnered with [Texas Advanced Computing Center (TACC)](https://www.tacc.utexas.edu/){target=_blank} where its data store is replicated nightly. US based researchers can request access to HPC via:

* [ACCESS-CI](https://access-ci.org/){target=_blank}

* [TACC Allocation request](https://portal.tacc.utexas.edu/allocations-overview){target=_blank}
    
## :octicons-database-24: Data Storage

CyVerse manages over 6 PB data via [iRODS (integrated Rule Oriented Data System)](https://irods.org){target=_blank} within two unique zones:

* `iplant` zone is the original project name, and is retained in production for all user accounts.

* `cyverse` zone is used in the CyVerse QA Environment

The data store is mirrored between University of Arizona and Texas Advanced Computing Center (TACC) nightly.

Resource Servers coordinate resources and contain physical storage devices. An iCAT server stores metadata in the form of “triples” to its relational database. 

## :material-web: Web Portals

[![][ball]{width=25}](https://user.cyverse.org/){target=_blank} [User Portal](https://user.cyverse.org){target=_blank} - a User Portal for creating and managing accounts, requesting and granting access to platforms, and a user management space for individuals and groups and workshops. 

[![][de]{width=25}](https://de.cyverse.org){target=_blank} [Discovery Environment](https://de.cyverse.org){target=_blank}  - Custom interactive web based data science workbench

[:material-shield-key: KeyCloak](https://kc.cyverse.org){target=_blank} - federated OAUTH to CyVerse resources, including Google, GitHub, ORCID,& CILogon

[![][data]{width=25}](https://data.cyverse.org){target=_blank} [WebDav](https://data.cyverse.org/){target=_blank} - `https://` endpoint for the iRODS data store

[![][data]{width=25}](){target=_blank} [SFTP](/){target=_blank} - built using SFTPGo

[![][data]{width=25}](https://datacommons.cyverse.org){target=_blank} [DataCommons](https://datacommons.cyverse.org/){target=_blank} - website hosting of published datasets in the CyVerse data store. The DataCommons presents any metadata which have been added by the owners to directories (folders) or files.

## :fontawesome-solid-staff-snake: Monitoring Services

[:material-web: Health Status](https://status.cyverse.org/){target=_blank} - system status monitor

[:material-crosshairs-gps: perfSONAR web toolkit](http://206.207.252.45/toolkit/){target=_blank} - network measurement toolkit
