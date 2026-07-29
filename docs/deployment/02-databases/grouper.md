---
type: Database
title: "Grouper database"
description: "Creating the database behind the Internet2 Grouper group-management service."
tags: [deployment, databases, grouper, groups]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

Holds Grouper's groups, folders, and memberships — the data the DE authorizes against.

# Create

Connect as a superuser on the host running
[PostgreSQL](../01-foundation/postgresql.md):

```bash
psql -h <DATABASE_HOST> -U postgres
```

```sql
-- as a superuser
create user grouper with password '<GENERATED_SECRET>';
create database grouper with owner grouper;
```

# Extensions

```sql
\c grouper
create extension "uuid-ossp";
create extension "moddatetime";
create extension "btree_gist";
```

# Migrations

Grouper manages its own schema through its installer (`gsh` / the Grouper
installer image) rather than through golang-migrate. Run the schema step from the
Grouper distribution before starting `grouper-loader`; see
[Grouper deployment](../05-core-services/grouper.md).


# Related

* [PostgreSQL](../01-foundation/postgresql.md)
* [Database migrations](./migrations.md)
* [Service databases](./index.md)
