---
type: Database
title: "Portal database"
description: "Creating, restoring, and seeding the User Portal database."
tags: [deployment, databases, user-portal]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: portal2
    resource: https://gitlab.com/cyverse/portal2
    title: CyVerse User Portal (portal2)
    author: team:cyverse
  - id: grid
    resource: https://www.grid.ac/
    title: GRID institution identifiers
---

Backs the [User Portal](../06-applications/user-portal.md): accounts, access
requests, workshops, form submissions, and the reference tables the signup forms
are built from.

# Create

Connect as a superuser on the host running
[PostgreSQL](../01-foundation/postgresql.md):

```bash
psql -h <DATABASE_HOST> -U postgres
```

```sql
create user portal_db_reader with password '<GENERATED_SECRET>';
create database portal with owner portal_db_reader;
```

The portal's own setup notes also grant the role membership in `postgres`:

```sql
GRANT postgres TO portal_db_reader;
```

!!! warning "That grant is broader than it looks"

    Membership in `postgres` gives the portal role superuser-equivalent reach over
    every database in the instance, including the iCAT. Grant it only if a portal
    migration actually needs it, revoke it afterwards, and prefer granting the
    specific privileges the portal needs. See
    [portal2](https://gitlab.com/cyverse/portal2)[^portal2] for what the
    application itself requires.

# Restore the base schema

The portal ships a SQL dump rather than an incremental migration history for the
initial load:

```bash
psql -U portal_db_reader -d portal -f portal.sql
```

`portal.sql` comes from the [portal2](https://gitlab.com/cyverse/portal2)
repository.

## Session table

The portal stores sessions in the database. Confirm the table exists after the
restore:

```sql
CREATE TABLE public.session (
    sid character varying NOT NULL,
    sess json NOT NULL,
    expire timestamp(6) without time zone NOT NULL
);
ALTER TABLE public.session OWNER TO portal;
CREATE INDEX "IDX_session_expire" ON public.session USING btree (expire);
```

!!! note "Two roles appear in the portal's own SQL"

    The dump creates the database owned by `portal_db_reader` but assigns the
    session table to `portal`. Reconcile these against the roles your deployment
    actually uses before the portal starts, or session writes fail with a
    permission error at first sign-in.

# Seed reference data

## Institutions

Institution autocomplete is seeded from the GRID dataset.[^grid] Download a
release, unzip it, and import with the script from the portal repository:

```bash
./import_grid_institutions.py \
    --host <DATABASE_HOST> --user portal_db_reader --database portal grid.csv
```

The script lives at `src/scripts/import_grid_institutions.py` in
[portal2](https://gitlab.com/cyverse/portal2).

## Form reference tables

The signup and profile forms read from a set of lookup tables. Load each one:

```bash
for table in country region gender occupation ethnicity \
             fundingagency awarechannel researcharea; do
  psql -U portal_db_reader -d portal -f "./account_${table}.sql"
done
```

The SQL files are published at
[portal2-db](https://github.com/cyverse-austria/portal2-db).

# Administrative queries

Promote an existing account to portal administrator:

```sql
UPDATE account_user SET is_superuser = true WHERE username = '<USERNAME>';
UPDATE account_user SET is_staff     = true WHERE username = '<USERNAME>';
```

Check or force email verification:

```sql
SELECT has_verified_email FROM account_user WHERE username = '<USERNAME>';
UPDATE account_user SET has_verified_email = true WHERE username = '<USERNAME>';
```

Prefer the [admin panel](../../operations/user-portal.md) for routine work. These
queries exist for bootstrapping the first administrator and for repairing accounts
the UI cannot reach — the normal path is
[bootstrap](../07-post-install/bootstrap.md).

# Migrations

Ongoing schema changes come from the portal application itself. See
[database migrations](./migrations.md) for the shared procedure.

# Related

* [User Portal deployment](../06-applications/user-portal.md)
* [User Portal administration](../../operations/user-portal.md)
* [PostgreSQL](../01-foundation/postgresql.md)

[^portal2]: CyVerse User Portal (portal2)
[^grid]: GRID institution identifiers
