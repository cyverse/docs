---
type: Database
title: "DE database"
description: "Creating and migrating the Discovery Environment database."
tags: [deployment, databases, discovery-environment]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

The core Discovery Environment database. It holds apps, tools, analyses, and — since the merge of the former `permissions` database — permissions as well.

# Create

Connect as a superuser on the host running
[PostgreSQL](../01-foundation/postgresql.md):

```bash
psql -h <DATABASE_HOST> -U postgres
```

```sql
-- as a superuser
create user de with password '<GENERATED_SECRET>';
create database de with owner de;
```

# Extensions

```sql
\c de
create extension "uuid-ossp";
create extension "moddatetime";
create extension "btree_gist";
```

# Populate and migrate

Schema and data come from [de-database](https://github.com/cyverse-de/de-database), applied with the shared procedure in
[database migrations](./migrations.md). In a normal deployment the
`setup-databases` and `update-databases` Ansible tags do this for you.

# Legacy version table

Deployments created before the move to golang-migrate carry a `version` table that
newer migrations do not create:

```sql
SET search_path = public, pg_catalog;

CREATE TABLE version (
    version character varying(20) NOT NULL,
    applied timestamp DEFAULT now()
);
```

It was seeded from `old-databases/de-db/src/main/data/999_version.sql` in the
de-database repository. A new deployment does not need it; it is documented here so
that finding it in an existing database is not a surprise.


# Related

* [PostgreSQL](../01-foundation/postgresql.md)
* [Database migrations](./migrations.md)
* [Service databases](./index.md)
