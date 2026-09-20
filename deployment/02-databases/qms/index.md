---
type: Database
title: "QMS database"
description: "Creating and migrating the quota management service database."
tags: [deployment, databases, qms, quotas]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

Backs the quota management service: subscription plans, resource quotas, and recorded usage.

# Create

Connect as a superuser on the host running
[PostgreSQL](https://docs.cyverse.org/deployment/01-foundation/postgresql/):

```bash
psql -h <DATABASE_HOST> -U postgres
```

```sql
-- as a superuser; owned by the de role
create database qms with owner de;
```

# Extensions

```sql
\c qms
create extension "uuid-ossp";
create extension "moddatetime";
create extension "btree_gist";
create extension "insert_username";
```

# Populate and migrate

Schema and data come from [QMS](https://github.com/cyverse/QMS) (`prod` branch), applied with the shared procedure in
[database migrations](https://docs.cyverse.org/deployment/02-databases/migrations/). In a normal deployment the
`setup-databases` and `update-databases` Ansible tags do this for you.

!!! warning "Connect to the right database first"

    Older notes run `\c de` before creating these extensions, which installs them
    into the DE database and leaves `qms` without them. The subsequent migration
    then fails on a missing function. Connect to `qms`, as above.


# Related

* [PostgreSQL](https://docs.cyverse.org/deployment/01-foundation/postgresql/)
* [Database migrations](https://docs.cyverse.org/deployment/02-databases/migrations/)
* [Service databases](https://docs.cyverse.org/deployment/02-databases/)
