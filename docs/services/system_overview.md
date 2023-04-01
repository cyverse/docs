
[de]: ../assets/de/deIcon.svg
[data]: ../assets/de/dataIcon.svg
[cacao]: ../assets/de/cacao-04.png
[ball]: ../assets/de/cyverse_ball_2022.png

CyVerse provides the Infrastructure as Code (IaC) necessary to manage a full stack cyberinfrastructure. The US public CyVerse primarily runs on hardware located at The University of Arizona, with a full data store mirror at the Texas Advanced Computing Center (TACC), and federated storage and compute resources located across the US. 

The full CyVerse stack can be deployed either on-premises consumer hardware or on cloud resources. 

Data storage is managed by an iRODS [![data]{width=25} Data Store](ds.md). 

Computing can be done in either the [![de]{width=25} Discovery Environment (DE)](de.md) data science workbench or with the [![cacao]{width=25} CACAO IaC](cloud.md) which leverages both public research computing and commercial cloud. 

Event-based triggers are accomplished through the DataWatch API.

<figure markdown>
  ![ecosystem](../assets/ecosystem.svg){width=800}
  <figcaption>CyVerse's Infrastructure as Code (IaC) provides computing, storage, and event-based components researchers rely upon for data intensive science.
</figcaption>
</figure>

## :material-api: Application Programming Interfaces (APIs)

All CyVerse APIs are [:simple-openapiinitiative: OpenAPI](https://www.openapis.org/) compliant.

[:material-terrain: Terrain API](https://de.cyverse.org/terrain/docs/index.html){target=_blank} is the main API for Discovery Environment and uses a [:simple-swagger: Swagger](https://swagger.io/) interface. 

* [:simple-jupyter: Terrain API Jupyter Notebooks](https://github.com/cyverse/terrain-notebook) - provide an introduction to Terrain and show how to start and stop analyses.

* [:simple-swagger: https://de.cyverse.org/terrain/swagger.json](https://de.cyverse.org/terrain/swagger.json)

[![cacao]{width=25} CACAO API](https://gitlab.com/cyverse/cacao/-/blob/master/docs/openapi/openapi.yaml){target=_blank} - Infrastructure as Code API for cloud automation with OpenAPI

[Data Watch API](https://gitlab.com/cyverse/datawatch/-/blob/master/docs/openapi/datawatch-openapi.yaml){target=_blank} - event based triggers for workflows with OpenAPI

CyVerse public-facing APIs are frequently leveraged by "[Powered-by-CyVerse](https://cyverse.org/powered-by-cyverse)" projects which utilize specific parts of the platform. 

## :octicons-cloud-24: Cloud Services

[![][cacao]{width=25}](https://cyverse.org/cacao){target=_blank} [Continous Automation / Continuous Analysis & Orchestration (CACAO)](https://cyverse.org/cacao){target=_blank} - Infrastructure as Code for multi-cloud deployments

* [:simple-terraform: CACAO Terraform Templates](https://gitlab.com/cyverse/cacao-tf-os-ops/){target=_blank}

* [:octicons-stopwatch-24: DataWatch](https://gitlab.com/cyverse/datawatch){target=_blank} - a notification system for reporting data events

## :material-server: Compute Resources

The DE runs on-premises hardware located at University of Arizona (UArizona) in the UITS colocation space at the high performance computing center. The data store is mirrored nightly at TACC. 

CyVerse staff maintain over XXX servers at UArizona, and XX servers at TACC.

Hardware is added, replaced, or upgraded every few months. Table values below may not be up-to-date.

**Primary Hardware Specifications**

Compute Nodes (XXX nodes)

| System Configuration | Aggregate information | Per Node (Compute Node) |
|----------------------|-----------------------|-------------------------|
| Machine types | Dell, SuperMicro, XXX |	 |
| Operating Systems | Centos, Rocky |	Centos, Rocky |
| Processor cores |	XX,XXX |	average XX |
| CPUs | 128, 64, 40, 32, 16 | 1, 2 |
| RAM | XXX TiB |	256, 128, 64, 32 GiB |
| Network |	100 Gbps to Internet2 | 10 Gpbs to switch |
| Storage |	X PB | X TB |

GPU Nodes (XXX nodes)

| System Configuration | Aggregate information | Per Node (Compute Node) |
|----------------------|-----------------------|-------------------------|
| Machine types | Dell, SuperMicro, XXX |	 |
| Operating Systems | Centos, Rocky |	Centos, Rocky |
| Processor cores |	 |	256 |
| CPUs | | 2 |
| RAM | |	1 TB, 512 GB |
| GPUs | NVIDIA (A100 80GB), (Tesla T4 16GB) | 4 |
| Network |	100 Gbps to Internet2 | 10 Gpbs to switch |
| Storage |	XXX TB | 28 TB SSD, 21 TB NVMe|

Storage Resource Nodes (XX nodes)

| System Configuration | Aggregate information | Per Node (Compute Node) |
|----------------------|-----------------------|-------------------------|
| Machine types | Dell, SuperMicro, XXX | |
| Operating Systems | Centos, Rocky |	Centos, Rocky |
| Processor cores |	XX,XXX |	average XX |
| CPUs | 128, 64, 40, 32, 16 | 1, 2 |
| RAM | XXX TiB |	256, 128, 64, 32 GiB |
| Network |	100 Gbps to Internet2 | 10 Gpbs to switch |
| Storage |	X PB | X TB |

### :simple-kubernetes: Federated Kubernetes Clusters

* CyVerse runs mainly on a locally managed K8s cluster, but it can be federated to other K8s clusters.

* The [National Research Platform](https://nationalresearchplatform.org/){target=_blank} offers federated K8s resources. These resources are currently in development.

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

![datastore](../assets/datastore.svg){width=500}

## :material-web: Interfaces

[![][ball]{width=25}](https://user.cyverse.org/){target=_blank} [User Portal](https://user.cyverse.org){target=_blank} - a User Portal for creating and managing accounts, requesting and granting access to platforms, and a user management space for individuals and groups and workshops. 

[![][de]{width=25}](https://de.cyverse.org){target=_blank} [Discovery Environment](https://de.cyverse.org){target=_blank}  - Custom interactive web based data science workbench

[:material-shield-key: KeyCloak](https://kc.cyverse.org){target=_blank} - federated OAUTH to CyVerse resources, including Google, GitHub, ORCID,& CILogon

[![][data]{width=25}](https://data.cyverse.org){target=_blank} [WebDav](https://data.cyverse.org/){target=_blank} - `https://` endpoint for the iRODS data store

[![][data]{width=25}](https://datacommons.cyverse.org){target=_blank} [DataCommons](https://datacommons.cyverse.org/){target=_blank} - website hosting of published datasets in the CyVerse data store. The DataCommons presents any metadata which have been added by the owners to directories (folders) or files.

## :fontawesome-solid-staff-snake: Monitoring Services

[:material-web: Health Status](https://status.cyverse.org/){target=_blank} - system status monitor

[:material-crosshairs-gps: perfSONAR web toolkit](http://206.207.252.45/toolkit/){target=_blank} - network measurement toolkit
