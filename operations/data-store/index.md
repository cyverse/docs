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
| iRODS protocol | Native access with `iCommands` or [GoCommands](https://github.com/cyverse/gocommands) | [Data Store](https://docs.cyverse.org/platform/data-store/) |
| WebDAV / HTTPS | `data.cyverse.org`, via Apache and davrods | [Data Store](https://docs.cyverse.org/platform/data-store/) |
| SFTP | SFTPGo with the iRODS backend | [Data Store](https://docs.cyverse.org/platform/data-store/) |
| CSI driver | Mounts collections into analysis pods | [iRODS CSI driver](https://docs.cyverse.org/deployment/05-core-services/irods-csi-driver/) |
| Terrain API | Programmatic filesystem operations | [filesystem endpoints](https://docs.cyverse.org/api/endpoints/filesystem/directory-listing/) |

Third-party clients that speak WebDAV or SFTP work against the endpoints above —
Cyberduck, FileZilla, and most desktop file managers among them. Nothing
CyVerse-specific has to be installed for those.

# Common administrative tasks

* **Sharing.** Permissions are set per collection or data object and can be
  granted to users or groups; see
  [permissions](https://docs.cyverse.org/api/endpoints/filesystem/permissions/) and
  [sharing](https://docs.cyverse.org/api/endpoints/filesystem/sharing/) for what the API exposes.
* **Anonymous access.** Public readability depends on the `anonymous` account's
  permissions, established during
  [zone initialization](https://docs.cyverse.org/deployment/03-data-store/irods-provider/#anonymous-access).
* **Community released folders.** Publishing a folder to all CyVerse users; see
  [Data Commons](https://docs.cyverse.org/platform/data-commons/).
* **Curated folders and DOIs.** Staff-reviewed publication with a DataCite DOI,
  driven by [permanent ID requests](https://docs.cyverse.org/api/endpoints/permanent-id-requests/).
  The reviewer's checklist is in
  [DE administration](https://docs.cyverse.org/operations/discovery-environment/).
* **Tickets.** Time- or use-limited anonymous access to a specific path; see
  [tickets](https://docs.cyverse.org/api/endpoints/filesystem/tickets/).

# Related

* [Data Store](https://docs.cyverse.org/platform/data-store/)
* [iRODS provider deployment](https://docs.cyverse.org/deployment/03-data-store/irods-provider/)
* [FAQ](https://docs.cyverse.org/operations/faq/)
