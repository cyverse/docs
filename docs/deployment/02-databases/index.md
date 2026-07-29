# Phase 2: databases

One database per service, all in the PostgreSQL instance from
[phase 1](../01-foundation/postgresql.md). In a normal deployment the
`setup-databases` and `update-databases` Ansible tags create and migrate the DE's
databases for you; these documents describe what those tags produce and how to do
it by hand.

* [Database migrations](migrations.md) - the shared golang-migrate procedure and shared extensions

# Data Store

* [iCAT database](icat.md) - the iRODS catalog, created by the iRODS installer

# Discovery Environment

* [DE database](de.md) - apps, tools, analyses, and permissions
* [Metadata database](metadata.md) - metadata templates and AVUs
* [Notifications database](notifications.md) - user notifications and system messages
* [QMS database](qms.md) - quotas, plans, and recorded usage

# Platform services

* [Keycloak database](keycloak.md) - realms, clients, and sessions
* [Grouper database](grouper.md) - groups, folders, and memberships
* [Unleash database](unleash.md) - feature flags
* [Portal database](portal.md) - accounts, requests, workshops, and form data

# Next

* [Phase 3: Data Store](../03-data-store/)
