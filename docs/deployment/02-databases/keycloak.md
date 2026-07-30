---
type: Database
title: "Keycloak database"
description: "Creating the database Keycloak uses for realms, clients, and sessions."
tags: [deployment, databases, keycloak]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

Holds Keycloak's realms, clients, roles, and sessions. Losing it means reconfiguring every realm and client by hand, so it is one of the databases most worth backing up.

# Create

Connect as a superuser on the host running
[PostgreSQL](../01-foundation/postgresql.md):

```bash
psql -h <DATABASE_HOST> -U postgres
```

```sql
-- as a superuser
create user keycloak with password '<GENERATED_SECRET>';
create database keycloak with owner keycloak;
```

# Migrations

Keycloak applies its own schema migrations at startup, so there is nothing to run
here. The first start after a Keycloak version upgrade takes noticeably longer for
that reason.

No extensions are required.


# Related

* [PostgreSQL](../01-foundation/postgresql.md)
* [Database migrations](./migrations.md)
* [Service databases](./index.md)
