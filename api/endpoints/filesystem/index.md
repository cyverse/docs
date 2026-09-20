# Filesystem endpoints

Terrain's view of the [Data Store](https://docs.cyverse.org/platform/data-store/). Paths are
iRODS paths, and every operation is subject to iRODS permissions.

# Browsing

* [Root listing](https://docs.cyverse.org/api/endpoints/filesystem/root-listing/) - top-level directories visible to the caller
* [Directory listing](https://docs.cyverse.org/api/endpoints/filesystem/directory-listing/) - non-recursive listings with paging
* [Stat](https://docs.cyverse.org/api/endpoints/filesystem/stat/) - status information for files and directories
* [Existence](https://docs.cyverse.org/api/endpoints/filesystem/existence/) - whether paths exist and are visible
* [Manifest](https://docs.cyverse.org/api/endpoints/filesystem/manifest/) - file manifest, including preview and infoType

# Reading content

* [Read chunk](https://docs.cyverse.org/api/endpoints/filesystem/read-chunk/) - byte ranges and pages without downloading
* [CSV/TSV parsing](https://docs.cyverse.org/api/endpoints/filesystem/csv-tsv-parsing/) - delimited files parsed into structured responses

# Modifying

* [Directory create](https://docs.cyverse.org/api/endpoints/filesystem/directory-create/) - create one or many directories
* [Move](https://docs.cyverse.org/api/endpoints/filesystem/move/) - move files and directories, individually or in bulk
* [Rename](https://docs.cyverse.org/api/endpoints/filesystem/rename/) - rename in place
* [Delete](https://docs.cyverse.org/api/endpoints/filesystem/delete/) - move to trash or delete outright
* [Restore](https://docs.cyverse.org/api/endpoints/filesystem/restore/) - restore from a user's trash
* [Empty trash](https://docs.cyverse.org/api/endpoints/filesystem/empty-trash/) - permanently empty the trash

# Metadata and search

* [Metadata](https://docs.cyverse.org/api/endpoints/filesystem/metadata/) - read, set, and copy AVU metadata
* [Search](https://docs.cyverse.org/api/endpoints/filesystem/search/) - search by name, metadata, and permissions

# Sharing

* [Permissions](https://docs.cyverse.org/api/endpoints/filesystem/permissions/) - list and update user permissions
* [Sharing](https://docs.cyverse.org/api/endpoints/filesystem/sharing/) - share and unshare with other users
* [Tickets](https://docs.cyverse.org/api/endpoints/filesystem/tickets/) - time- or use-limited anonymous access

# Integrations and formats

* [CoGe](https://docs.cyverse.org/api/endpoints/filesystem/coge/) - expose genome files to CoGe
* [OAI-ORE](https://docs.cyverse.org/api/endpoints/filesystem/ore/) - generate OAI-ORE descriptions of a dataset
* [Path lists](https://docs.cyverse.org/api/endpoints/filesystem/path-lists/) - build HT path list files

# Errors

* [Filesystem errors](https://docs.cyverse.org/api/endpoints/filesystem/errors/) - error codes returned by these endpoints
