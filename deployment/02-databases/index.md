# Phase 2: databases

One database per service, all in the PostgreSQL instance from
[phase 1](https://docs.cyverse.org/deployment/01-foundation/postgresql/). In a normal deployment the
`setup-databases` and `update-databases` Ansible tags create and migrate the DE's
databases for you; these documents describe what those tags produce and how to do
it by hand.

* [Database migrations](https://docs.cyverse.org/deployment/02-databases/migrations/) - the shared golang-migrate procedure and shared extensions

# Data Store

* [iCAT database](https://docs.cyverse.org/deployment/02-databases/icat/) - the iRODS catalog, created by the iRODS installer

# Discovery Environment

* [DE database](https://docs.cyverse.org/deployment/02-databases/de/) - apps, tools, analyses, and permissions
* [Metadata database](https://docs.cyverse.org/deployment/02-databases/metadata/) - metadata templates and AVUs
* [Notifications database](https://docs.cyverse.org/deployment/02-databases/notifications/) - user notifications and system messages
* [QMS database](https://docs.cyverse.org/deployment/02-databases/qms/) - quotas, plans, and recorded usage

# Platform services

* [Keycloak database](https://docs.cyverse.org/deployment/02-databases/keycloak/) - realms, clients, and sessions
* [Grouper database](https://docs.cyverse.org/deployment/02-databases/grouper/) - groups, folders, and memberships
* [Unleash database](https://docs.cyverse.org/deployment/02-databases/unleash/) - feature flags
* [Portal database](https://docs.cyverse.org/deployment/02-databases/portal/) - accounts, requests, workshops, and form data

# Next

* [Phase 3: Data Store](https://docs.cyverse.org/deployment/03-data-store)
