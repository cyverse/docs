---
type: Deployment Procedure
title: "Database migrations"
description: "The shared golang-migrate procedure used to create and update every CyVerse service schema."
tags: [deployment, databases, migrations, postgresql]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: golang-migrate
    resource: https://github.com/golang-migrate/migrate
    title: golang-migrate
    author: team:golang-migrate
---

# Two ways to migrate

Every CyVerse schema is versioned as a directory of `migrations` applied with
[golang-migrate](https://github.com/golang-migrate/migrate).[^golang-migrate] You
will use one of two paths:

| Path | When |
|------|------|
| Ansible tags | Normal deployments and upgrades — the playbooks migrate every DE database in one pass |
| `migrate` by hand | Bootstrapping a single database, or debugging a failed migration |

## With Ansible

```bash
# create the databases
ansible-playbook -i /path/to/inventory --tags setup-databases   kubernetes.yml

# apply outstanding migrations
ansible-playbook -i /path/to/inventory --tags update-databases  kubernetes.yml
```

Run `update-databases` after every service upgrade. It is idempotent: with nothing
outstanding it reports no changes.

## By hand

Install `migrate` once. On Debian and Ubuntu:

```bash
curl -s https://packagecloud.io/install/repositories/golang-migrate/migrate/script.deb.sh | sudo bash
apt-get update
apt-get install -y migrate
migrate -help
```

Then, from a checkout of the repository that owns the schema:

```bash
migrate -database "postgres://<USER>:<GENERATED_SECRET>@<DATABASE_HOST>/<DBNAME>?sslmode=disable" \
        -path migrations up
```

!!! warning "`sslmode=disable` and credentials on the command line"

    Both appear in existing CyVerse notes and both are compromises. Prefer
    `sslmode=require` where the server supports it, and put the URL in an
    environment variable rather than in the command, so the password stays out of
    shell history and process listings.

# Where the migrations live

| Database | Repository |
|----------|------------|
| `de`, `notifications`, `metadata` | [de-database](https://github.com/cyverse-de/de-database) |
| `qms` | [QMS](https://github.com/cyverse/QMS) (`prod` branch) |
| `portal` | [portal2](https://gitlab.com/cyverse/portal2) |
| `unleash` | Applied by Unleash itself at startup |
| `keycloak` | Applied by Keycloak itself at startup |
| `grouper` | Applied by the Grouper installer |

# Shared extensions

Most CyVerse databases need the same extensions, created as a superuser in the
target database before migrating:

```sql
\c <DBNAME>
create extension "uuid-ossp";
create extension "moddatetime";
create extension "btree_gist";
```

The QMS database also needs `insert_username`. A migration that fails on a missing
function almost always means an extension was not created first.

# Troubleshooting

**Migrations reported as applied but the schema is wrong.** Check
`standard_conforming_strings`; when it is on, some DE migrations are skipped. See
[troubleshooting](../07-post-install/troubleshooting.md).

**A migration failed halfway.** `migrate` records a dirty version. Inspect
`schema_migrations`, fix the cause, then force the version back to the last good
one before re-running — do not delete the table.

# Related

* [PostgreSQL](../01-foundation/postgresql.md)
* [Service databases](./index.md)

[^golang-migrate]: golang-migrate
