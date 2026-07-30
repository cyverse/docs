---
type: Playbook
title: "Data Store administration"
description: "Client tooling, sharing, and curation workflows for the iRODS Data Store."
tags: [operations, administration, data-store, irods]
status: draft
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

!!! warning "Outline, not a complete guide"

    This document was a list of empty headings. It now records what each topic
    covers and points at the authoritative source; the procedures themselves are
    still to be written. User-facing instructions live at
    [learning.cyverse.org](https://learning.cyverse.org/){target=_blank}.

# Access paths

| Path | What it is | Reference |
|------|------------|-----------|
| iRODS protocol | Native access with `iCommands` or [GoCommands](https://github.com/cyverse/gocommands) | [Data Store](../platform/data-store.md) |
| WebDAV / HTTPS | `data.cyverse.org`, via Apache and davrods | [Data Store](../platform/data-store.md) |
| SFTP | SFTPGo with the iRODS backend | [Data Store](../platform/data-store.md) |
| CSI driver | Mounts collections into analysis pods | [iRODS CSI driver](../deployment/05-core-services/irods-csi-driver.md) |
| Terrain API | Programmatic filesystem operations | [filesystem endpoints](../api/endpoints/filesystem/directory-listing.md) |

Third-party clients that speak WebDAV or SFTP work against the endpoints above —
Cyberduck, FileZilla, and most desktop file managers among them. Nothing
CyVerse-specific has to be installed for those.

# Common administrative tasks

* **Sharing.** Permissions are set per collection or data object and can be
  granted to users or groups; see
  [permissions](../api/endpoints/filesystem/permissions.md) and
  [sharing](../api/endpoints/filesystem/sharing.md) for what the API exposes.
* **Anonymous access.** Public readability depends on the `anonymous` account's
  permissions, established during
  [zone initialization](../deployment/03-data-store/irods-provider.md#anonymous-access).
* **Community released folders.** Publishing a folder to all CyVerse users; see
  [Data Commons](../platform/data-commons.md).
* **Curated folders and DOIs.** Staff-reviewed publication with a DataCite DOI,
  driven by [permanent ID requests](../api/endpoints/permanent-id-requests.md).
  The reviewer's checklist is in
  [DE administration](./discovery-environment.md).
* **Tickets.** Time- or use-limited anonymous access to a specific path; see
  [tickets](../api/endpoints/filesystem/tickets.md).

# Related

* [Data Store](../platform/data-store.md)
* [iRODS provider deployment](../deployment/03-data-store/irods-provider.md)
* [FAQ](./faq.md)
