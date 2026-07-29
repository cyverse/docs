# Filesystem endpoints

Terrain's view of the [Data Store](../../../platform/data-store.md). Paths are
iRODS paths, and every operation is subject to iRODS permissions.

# Browsing

* [Root listing](root-listing.md) - top-level directories visible to the caller
* [Directory listing](directory-listing.md) - non-recursive listings with paging
* [Stat](stat.md) - status information for files and directories
* [Existence](existence.md) - whether paths exist and are visible
* [Manifest](manifest.md) - file manifest, including preview and infoType

# Reading content

* [Read chunk](read-chunk.md) - byte ranges and pages without downloading
* [CSV/TSV parsing](csv-tsv-parsing.md) - delimited files parsed into structured responses

# Modifying

* [Directory create](directory-create.md) - create one or many directories
* [Move](move.md) - move files and directories, individually or in bulk
* [Rename](rename.md) - rename in place
* [Delete](delete.md) - move to trash or delete outright
* [Restore](restore.md) - restore from a user's trash
* [Empty trash](empty-trash.md) - permanently empty the trash

# Metadata and search

* [Metadata](metadata.md) - read, set, and copy AVU metadata
* [Search](search.md) - search by name, metadata, and permissions

# Sharing

* [Permissions](permissions.md) - list and update user permissions
* [Sharing](sharing.md) - share and unshare with other users
* [Tickets](tickets.md) - time- or use-limited anonymous access

# Integrations and formats

* [CoGe](coge.md) - expose genome files to CoGe
* [OAI-ORE](ore.md) - generate OAI-ORE descriptions of a dataset
* [Path lists](path-lists.md) - build HT path list files

# Errors

* [Filesystem errors](errors.md) - error codes returned by these endpoints
