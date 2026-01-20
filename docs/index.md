![](assets/cyverse_logo_2022.svg)

# CyVerse Core Software Documentation

Welcome to the comprehensive technical documentation for deploying and maintaining the CyVerse cyberinfrastructure. This documentation is designed to support DevOps engineers, system administrators, and application developers working with CyVerse's open-source scientific cyberinfrastructure.

## Documentation Overview

CyVerse is a Software as a Service (SaaS) platform built on modern cloud-native technologies, providing researchers with powerful computational infrastructure for data-intensive science. Our documentation covers the complete stack from infrastructure deployment to API integration. 

<figure markdown>
  ![layercake](assets/layerCake.svg){width=800}
  <figcaption>The CyVerse infrastructure layer cake is stacked upon bare-metal Hardware, then Services, and finally Products</figcaption>
</figure>

---

## Find Documentation by Your Role

### :material-server: DevOps Engineer

**You're deploying and maintaining CyVerse infrastructure**

**Start here:**
- [Getting Started Guide](services/getting_started.md) - Prerequisites and deployment roadmap
- [System Overview](services/system_overview.md) - Architecture and technology stack
- [Deployment Guide](deployments/deployment_overview.md) - Complete deployment walkthrough

**Common tasks:**
- [Deploy Kubernetes cluster](deployments/kubernetes-deploy.md) - Core infrastructure setup
- [Configure authentication](deployments/keycloak.md) - KeyCloak deployment and LDAP integration
- [Set up storage](deployments/openebs.md) - OpenEBS and iRODS CSI driver
- [Deploy Discovery Environment](deployments/DiscoveryEnvironment.md) - Main application platform
- [Database management](database/main.md) - PostgreSQL schemas and migrations

### :octicons-people-24: System Administrator

**You're managing users, apps, and operational tasks**

**Start here:**
- [Administration Guide](guides/devops.md) - Daily operational procedures
- [Discovery Environment Admin](guides/de.md) - User management, app publishing, VICE access
- [Data Store Admin](guides/ds.md) - Data management and permissions

**Common tasks:**
- [Grant VICE access](guides/de.md#vice-access) - Enable interactive computing for users
- [Process DOI/Permanent ID requests](services/api/endpoints/permanent-id-requests.md) - Data publishing workflows
- [Manage user groups](deployments/grouper.md) - Grouper administration
- [Publish apps to Discovery Environment](guides/de.md#app-publication) - Tool integration
- [FAQ](guides/faq.md) - Frequently asked questions and troubleshooting

### :material-code-braces: Application Developer

**You're integrating with CyVerse APIs or contributing code**

**Start here:**
- [Developer Guide](development/index.md) - Development environment and contribution workflow
- [API Overview](services/api_overview.md) - Terrain API introduction
- [API Endpoint Index](services/api/endpoint-index.md) - Complete API reference

**Common tasks:**
- [Authentication](services/keycloak.md) - OAUTH 2.0 integration
- [Filesystem operations](services/api/endpoints/filesystem/directory-listing.md) - Data Store API
- [Job submission](services/api/endpoints/endpoints.md) - Analysis execution
- [Tapis v2 to v3 migration](services/api/tapis-v2-v3-migration.md) - Upgrade guide
- [Error handling](services/api/errors.md) - API error codes and responses

---

## Documentation Sections

| Section | Description | Audience |
|---------|-------------|----------|
| [**Getting Started**](services/getting_started.md) | Prerequisites, system overview, and deployment roadmap | DevOps, New users |
| [**Deployment Guide**](deployments/deployment_overview.md) | Complete infrastructure deployment: Kubernetes, services, databases | DevOps Engineers |
| [**Administration Guide**](guides/devops.md) | Operational procedures, user management, app publishing | System Administrators |
| [**API Reference**](services/api_overview.md) | Terrain API endpoints, authentication, error handling | Developers |
| [**Developer Guide**](development/index.md) | Development environment, contribution workflow, architecture | Developers |
| [**System Architecture**](services/services_overview.md) | Platform components, services, and design decisions | All audiences |
| [**Database Reference**](database/main.md) | PostgreSQL schemas for all CyVerse services | DevOps, DBAs |

---

## CyVerse Platform Components

### Core Products
- **[Discovery Environment (DE)](services/de.md)** - Web-based analysis platform for computational workflows
- **[Data Store](services/ds.md)** - 6+ PB iRODS-based data management system
- **[Data Commons](services/dc.md)** - Data publishing platform with DOI/ARK support
- **[VICE](deployments/vice.md)** - Visual Interactive Computing Environment (Jupyter, RStudio, etc.)

### Specialized Tools
- **[BisQue](services/bisque.md)** - Bio-Image Semantic Query User Environment
- **[DNA Subway](services/dnasubway.md)** - Educational genomics platform
- **[Cloud Services (CACAO)](services/cloud.md)** - Cloud automation and multi-cloud orchestration

### Infrastructure Services
- **[Authentication (KeyCloak)](services/keycloak.md)** - OAUTH 2.0, LDAP, CILogon integration
- **[Terrain API](services/api_overview.md)** - RESTful API aggregating all DE services
- **[Kubernetes](deployments/kubernetes-deploy.md)** - Container orchestration platform
- **[iRODS](deployments/irods-csi-driver.md)** - Integrated Rule-Oriented Data System for data management

---

## Quick Links

- :material-frequently-asked-questions: [**FAQ**](guides/faq.md) - Frequently asked questions and troubleshooting
- :simple-github: [**GitHub Organization**](https://github.com/cyverse-de){target=_blank} - Source code repositories
- :material-api: [**Live Terrain API**](https://de.cyverse.org/terrain/docs/){target=_blank} - Interactive API documentation
- :simple-docker: [**Harbor Registry**](https://harbor.cyverse.org/){target=_blank} - Container image repository
- :material-web: [**CyVerse Website**](https://cyverse.org){target=_blank} - Public-facing information and user support

---

??? Question "What is CyVerse?"

    [Public Website](https://cyverse.org){target=_blank}

    CyVerse is a powerful computational infrastructure and the people who support its operations. It is fully open source, and dedicated to furthering open science.
    
    CyVerse has been funded by the [United States National Science Foundation (NSF)](https://www.nsf.gov/){target=_blank} from 2008 until the present

    [![nsf](assets/NSF.svg){width=100}](https://www.nsf.gov/){target=_blank} 

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
