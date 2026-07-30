---
type: Service
title: "Data Commons"
description: "The publishing platform for curated and community-released datasets."
tags: [platform, data-commons, publishing, doi]
status: draft
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

# What it is

The [Data Commons](https://datacommons.cyverse.org) provides HTTP access to
datasets published from the CyVerse [Data Store](./data-store.md), together with
whatever metadata their owners attached. It is the read side of publishing: data
stays in iRODS, and the Data Commons presents it to people who do not have a
CyVerse account.

Published data is read through the `anonymous` iRODS account, which is why
[zone initialization](../deployment/03-data-store/irods-provider.md#anonymous-access)
grants that account read permission on the zone and on `/home`.

# Publishing paths

| Path | What it produces |
|------|------------------|
| Community released | A folder visible to all CyVerse users, with no persistent identifier |
| Curated | A curated dataset with a DataCite DOI, after staff review |

The DOI workflow is driven by permanent ID requests through Terrain — see
[permanent ID requests](../api/endpoints/permanent-id-requests.md) for the API, and
[DE administration](../operations/discovery-environment.md) for the review steps
curators follow.

!!! note "This document is a stub"

    It records what the Data Commons is and how it connects to the rest of the
    platform. The curation workflow lives in the operations guide, and there is no
    Data Commons–specific deployment component beyond the Data Store's access
    services.

# Related

* [Data Store](./data-store.md)
* [Permanent ID requests](../api/endpoints/permanent-id-requests.md)
* [Data Store administration](../operations/data-store.md)
