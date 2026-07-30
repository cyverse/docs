---
type: Database
title: "iCAT database"
description: "The iRODS catalog database: what creates it, what may read it, and what must never write to it."
tags: [deployment, databases, irods, icat]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: irods-install
    resource: https://docs.irods.org/4.3.3/getting_started/installation/
    title: iRODS 4.3.3 installation guide
    author: team:irods-consortium
---

# What it is

The iCAT is iRODS's catalog: every collection, data object, replica, AVU, ticket,
and permission in the zone. It is the one CyVerse database that is not managed by
CyVerse migrations — its schema belongs to iRODS.

# Who creates it

The iRODS installer, during
[iRODS provider setup](../03-data-store/irods-provider.md). You do not create it
by hand. What you do beforehand is prepare PostgreSQL for iRODS as the iRODS
project describes,[^irods-install] which creates the database, the `irods` role,
and its ODBC connectivity — see
[PostgreSQL](../01-foundation/postgresql.md#prepare-for-irods).

The database name is chosen at install time and recorded in
`/etc/irods/server_config.json`.

# Who may read it

The DE reads the catalog directly for some listings, through a PostgreSQL role
with `SELECT` on all iCAT tables and nothing more. That role is separate from the
`de-irods` iRODS account.

!!! danger "Nothing but iRODS writes to the iCAT"

    Writing to the catalog outside iRODS bypasses policy: no rules fire, no AVUs
    are maintained, no messages are published, and the vault and catalog can
    disagree. Every write goes through the iRODS protocol, including
    administrative ones.

# Backup

The iCAT and the vault have to be backed up as a pair. A catalog restored to a
different point in time than the vault leaves data objects registered that do not
exist, and files on disk that nothing can reach.

Include both in the operational readiness checks in
[verification](../07-post-install/verification.md).

# Related

* [iRODS catalog provider](../03-data-store/irods-provider.md)
* [PostgreSQL](../01-foundation/postgresql.md)
* [Data Store](../../platform/data-store.md)

[^irods-install]: iRODS 4.3.3 installation guide
