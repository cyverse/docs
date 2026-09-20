---
type: Database
title: "Metadata database"
description: "Creating the database behind the CyVerse metadata service."
tags: [deployment, databases, metadata]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

Holds user-defined metadata templates and the AVU metadata the metadata service manages on behalf of the DE.

# Create

Connect as a superuser on the host running
[PostgreSQL](https://docs.cyverse.org/deployment/01-foundation/postgresql/):

```bash
psql -h <DATABASE_HOST> -U postgres
```

```sql
-- as a superuser; owned by the de role, not a role of its own
create database metadata with owner de;
```

# Extensions

```sql
\c metadata
create extension "uuid-ossp";
create extension "moddatetime";
create extension "btree_gist";
```

# Populate and migrate

Schema and data come from [de-database](https://github.com/cyverse-de/de-database), applied with the shared procedure in
[database migrations](https://docs.cyverse.org/deployment/02-databases/migrations/). In a normal deployment the
`setup-databases` and `update-databases` Ansible tags do this for you.


# Related

* [PostgreSQL](https://docs.cyverse.org/deployment/01-foundation/postgresql/)
* [Database migrations](https://docs.cyverse.org/deployment/02-databases/migrations/)
* [Service databases](https://docs.cyverse.org/deployment/02-databases/)
