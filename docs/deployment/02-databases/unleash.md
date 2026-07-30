---
type: Database
title: "Unleash database"
description: "Creating the database behind the Unleash feature-flag service."
tags: [deployment, databases, unleash, feature-flags]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

Holds the DE's feature toggles, including the maintenance flag.

# Create

Connect as a superuser on the host running
[PostgreSQL](../01-foundation/postgresql.md):

```bash
psql -h <DATABASE_HOST> -U postgres
```

```sql
-- as a superuser
create user unleash with password '<GENERATED_SECRET>';
create database unleash with owner unleash;
```

# Extensions

```sql
\c unleash
create extension "uuid-ossp";
create extension "moddatetime";
create extension "btree_gist";
```

# Migrations

Unleash applies its own schema migrations at startup; there is nothing to run here.
See [Unleash deployment](../05-core-services/unleash.md).


# Related

* [PostgreSQL](../01-foundation/postgresql.md)
* [Database migrations](./migrations.md)
* [Service databases](./index.md)
