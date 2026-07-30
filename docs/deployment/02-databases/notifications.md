---
type: Database
title: "Notifications database"
description: "Creating the database behind the user notification service."
tags: [deployment, databases, notifications]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

Holds user notifications and system messages, including the read and seen state the DE displays.

# Create

Connect as a superuser on the host running
[PostgreSQL](../01-foundation/postgresql.md):

```bash
psql -h <DATABASE_HOST> -U postgres
```

```sql
-- as a superuser; owned by the de role
create database notifications with owner de;
```

# Extensions

```sql
\c notifications
create extension "uuid-ossp";
create extension "moddatetime";
create extension "btree_gist";
```

# Populate and migrate

Schema and data come from [de-database](https://github.com/cyverse-de/de-database), applied with the shared procedure in
[database migrations](./migrations.md). In a normal deployment the
`setup-databases` and `update-databases` Ansible tags do this for you.


# Related

* [PostgreSQL](../01-foundation/postgresql.md)
* [Database migrations](./migrations.md)
* [Service databases](./index.md)
