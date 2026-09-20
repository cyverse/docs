---
type: Reference
title: "About this documentation"
description: "What CyVerse is, who this documentation is for, and where each audience should start."
tags: [about, orientation]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: okf-spec
    resource: https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md
    title: Open Knowledge Format v0.2 specification
    author: team:google-cloud-platform
    last_modified: 2026-07-29
---

![](https://docs.cyverse.org/assets/cyverse_logo_2022.svg)

# What CyVerse is

CyVerse is a computational infrastructure for data-intensive science, and the
people who operate it. It is fully open source and funded by the
[United States National Science Foundation](https://www.nsf.gov/){target=_blank}.

It is both a Software as a Service platform and the Infrastructure as Code needed
to run one: the same stack that serves the public US deployment can be deployed by
another institution on its own hardware or in the cloud. That is what this
documentation is for.

<figure markdown>
  ![layercake](https://docs.cyverse.org/assets/layerCake.svg){width=800}
  <figcaption>Hardware at the bottom, services in the middle, products on top</figcaption>
</figure>

# Who this is for

## Deploying CyVerse

Start with [prerequisites](https://docs.cyverse.org/deployment/planning/prerequisites/), then read
[deploying from scratch](https://docs.cyverse.org/deployment/from-scratch/) end to end before running
anything. Work the phases in [deployment](https://docs.cyverse.org/deployment) in order, and check each one
against [verification](https://docs.cyverse.org/deployment/07-post-install/verification/).

Before provisioning: [component inventory and
sizing](https://docs.cyverse.org/architecture/component-inventory/) and [network
requirements](https://docs.cyverse.org/architecture/network-requirements/).

## Operating a deployment

[operations/](https://docs.cyverse.org/operations) covers day-to-day administration: users and VICE access
in [DE administration](https://docs.cyverse.org/operations/discovery-environment/), data and curation in
[Data Store administration](https://docs.cyverse.org/operations/data-store/), accounts in [User Portal
administration](https://docs.cyverse.org/operations/user-portal/), and the recurring questions in the
[FAQ](https://docs.cyverse.org/operations/faq/).

## Integrating with the APIs

[Terrain](https://docs.cyverse.org/api/terrain/) is the API behind every CyVerse product. The
[endpoint index](https://docs.cyverse.org/api/endpoint-index/) lists everything documented here, and the
live [Swagger reference](https://de.cyverse.org/terrain/docs/){target=_blank} is
the most current source. Authentication is [OAuth 2.0 through
Keycloak](https://docs.cyverse.org/platform/authentication/).

## Contributing code

[development/](https://docs.cyverse.org/development) covers the development environment and contribution
workflow. Source lives in the
[CyVerse](https://github.com/cyverse){target=_blank} and
[CyVerse DE](https://github.com/cyverse-de){target=_blank} GitHub organizations.

# What CyVerse offers its users

| Product | What it does |
|---------|--------------|
| [Discovery Environment](https://docs.cyverse.org/platform/discovery-environment/) | Web-based data science workbench with hundreds of integrated tools |
| [Data Store](https://docs.cyverse.org/platform/data-store/) | Multi-petabyte iRODS storage with HTTPS, WebDAV, SFTP, and API access |
| [Data Commons](https://docs.cyverse.org/platform/data-commons/) | Publishing curated and community-released datasets, with DataCite DOIs |
| VICE | Interactive computing — JupyterLab, RStudio, Shiny — inside the DE |
| [Cloud services (CACAO)](https://docs.cyverse.org/platform/cloud/) | Infrastructure as code for multi-cloud deployments |
| [BisQue](https://docs.cyverse.org/platform/bisque/) | Bio-image semantic query and analysis |
| [DNA Subway](https://docs.cyverse.org/platform/dna-subway/) | Educational genomics workflows |

# How this documentation is organized

This bundle follows the [Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
v0.2.[^okf-spec] In practice that means three things you can rely on:

* **Every document declares itself.** Frontmatter carries its `type`, a one-line
  `description`, `tags`, and a `status` of `draft`, `stable`, or `deprecated`. A
  `draft` document is incomplete and says so rather than pretending otherwise.
* **Every directory has an index.** `index.md` lists what is in a directory with a
  line of description each, so you can see what exists before opening anything.
* **Derived documents cite their sources.** Where a document was written from
  something else, `sources` in its frontmatter says what, including material
  mirrored under [references/](https://docs.cyverse.org/references).

Changes to the bundle are recorded in [the log](https://docs.cyverse.org/log/).

# Links

* :material-web: [CyVerse website](https://cyverse.org){target=_blank}
* :material-frequently-asked-questions: [FAQ](https://docs.cyverse.org/operations/faq/)
* :simple-github: [GitHub organization](https://github.com/cyverse-de){target=_blank}
* :material-api: [Live Terrain API](https://de.cyverse.org/terrain/docs/){target=_blank}
* :simple-docker: [Harbor registry](https://harbor.cyverse.org/){target=_blank}
* :material-school: [User-facing learning materials](https://learning.cyverse.org/){target=_blank}

# Funding

[![nsf](https://docs.cyverse.org/assets/NSF.svg){width=100}](https://www.nsf.gov/){target=_blank}

CyVerse has been funded by the National Science Foundation from 2008 to the
present.

[![NSF-0735191](https://img.shields.io/badge/NSF-0735191-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=0735191) [![NSF-1265383](https://img.shields.io/badge/NSF-1265383-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1265383) [![NSF-1743442](https://img.shields.io/badge/NSF-1743442-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1743442)

!!! Info ":fontawesome-brands-creative-commons-by: SOFTWARE LICENSE"

    Copyright (c) 2010-2026, The Arizona Board of Regents on behalf of The University of Arizona

    All rights reserved.

    Developed by: CyVerse as a collaboration between participants at BIO5 at The University of Arizona (the primary hosting institution), Cold Spring Harbor Laboratory, The University of Texas at Austin, and individual contributors. Find out more at http://www.cyverse.org/.

    Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

    * Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
    * Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
    * Neither the name of CyVerse, BIO5, The University of Arizona, Cold Spring Harbor Laboratory, The University of Texas at Austin, nor the names of other contributors may be used to endorse or promote products derived from this software without specific prior written permission.

    THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

[^okf-spec]: https://docs.cyverse.org/about/Open Knowledge Format v0.2 specification
