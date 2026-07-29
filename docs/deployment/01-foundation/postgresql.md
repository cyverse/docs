---
type: Deployment Procedure
title: "PostgreSQL"
description: "Installing and tuning the PostgreSQL server that backs the iRODS catalog and every CyVerse service database."
tags: [deployment, foundation, postgresql, database]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: irods-install
    resource: https://docs.irods.org/4.3.3/getting_started/installation/
    title: iRODS 4.3.3 installation guide
    author: team:irods-consortium
  - id: postgres-docs
    resource: https://www.postgresql.org/docs/
    title: PostgreSQL documentation
    author: team:postgresql
---

# Role in the deployment

One PostgreSQL instance carries two very different workloads:

* the **iRODS catalog (iCAT)**, which is latency-sensitive and touched by every
  data operation, and
* the **DE service databases**, which are created and migrated by Ansible.

Because both live in the same instance in a pilot, tuning is sized against the
memory and cores you deliberately reserve for the database — not against the
machine's total capacity. Decide that reservation first, from
[component inventory](../../architecture/component-inventory.md) (the pilot
reserved 22 cores and 56 GB), then compute the settings below from it.

Separating the catalog onto its own instance is the first split most sites make
as they grow, because the DE and the catalog otherwise compete for the same
buffer cache.

# Host preparation

## Transparent huge pages

PostgreSQL uses explicit huge pages; transparent huge pages cause latency
spikes and have to be off.

1. Append `transparent_hugepage=never` to `GRUB_CMDLINE_LINUX_DEFAULT` in
   `/etc/default/grub`.
2. Run `update-grub` to write the boot loader configuration.
3. Reboot for the change to take effect.

## Kernel parameters

Persist these (`/etc/sysctl.d/`), do not just `sysctl -w` them:

| Parameter | Value |
|-----------|-------|
| `vm.nr_hugepages` | `0.6 × (memory_available_to_postgres_in_kiB ÷ 2048)` |
| `vm.swappiness` | `5` |

The huge pages figure reserves roughly 60% of the database's memory allocation
as 2 MiB pages, which is what `shared_buffers` plus overhead needs when
`huge_pages = on`.

## Packages

Install `postgresql`, `postgresql-client`, and `python3-psycopg2` — the last one
so Ansible's PostgreSQL modules can talk to the instance. The pilot ran
PostgreSQL 16 from the distribution packages, which puts configuration in
`/etc/postgresql/16/main/`.

# Tuning

Substitute your own reservation for `M` (memory available to PostgreSQL, in
kiB) and `C` (cores available to PostgreSQL):

| Setting | Value | Why |
|---------|-------|-----|
| `max_connections` | `300` | Estimated by the deployment team for the DE service set plus iRODS agents |
| `shared_buffers` | `M ÷ 4` kB | Conventional quarter-of-memory starting point |
| `huge_pages` | `on` | Pairs with `vm.nr_hugepages` above |
| `work_mem` | `150MB` | Starting point only; tune against real query plans |
| `maintenance_work_mem` | `2GB` | Keeps index builds and vacuums off disk |
| `effective_io_concurrency` | `200` | Assumes SSD or NVMe storage |
| `max_worker_processes` | `C` | |
| `max_parallel_maintenance_workers` | `C` | |
| `max_parallel_workers_per_gather` | `C` | |
| `max_parallel_workers` | `C` | |
| `checkpoint_timeout` | `15min` | Fewer, larger checkpoints |
| `max_wal_size` | `4GB` | |
| `min_wal_size` | `1GB` | |
| `random_page_cost` | `1.1` | Assumes SSD or NVMe storage |
| `effective_cache_size` | `M ÷ 2` kB | What the planner assumes the OS caches |
| `shared_preload_libraries` | `'pg_stat_statements'` | Query-level visibility |
| `pg_stat_statements.max` | `10000` | |
| `pg_stat_statements.track` | `all` | |
| `standard_conforming_strings` | `off` | Required by the DE database |
| `listen_addresses` | `'*'` | Needed for cluster pods and the other node |

!!! warning "`work_mem` is per sort, not per connection"

    With `max_connections = 300`, a plan with several concurrent sorts can
    multiply `work_mem` well past what you expect. Treat `150MB` as a starting
    point and lower it if the instance starts swapping.

!!! warning "`standard_conforming_strings = off`"

    The DE database expects this off. Leaving it on causes some migrations to be
    skipped; re-running the `update-databases` tag after fixing it picks them
    up. See [troubleshooting](../07-post-install/troubleshooting.md).

Restart PostgreSQL after editing `postgresql.conf`.

# Prepare for iRODS

Follow the iRODS project's instructions for preparing PostgreSQL for
iRODS,[^irods-install] which cover creating the catalog database, the `irods`
role, and its ODBC connectivity. Then create a role for the DE with a generated
password and `SELECT` on all tables in the catalog database — the DE reads the
catalog directly for some listings and never writes to it.

# Access control

`pg_hba.conf` is edited twice, in two different phases:

1. **Phase 2**, before the cluster exists: allow your admin host or hosts and
   both deployment nodes, with method `scram-sha-256`.
2. **Phase 4**, once the cluster exists: add the Kubernetes pod CIDR or CIDRs,
   because the DE services connect from pods:

   ```bash
   kubectl get nodes -o jsonpath='{.items[*].spec.podCIDR}' && echo
   ```

   For each distinct CIDR add:

   ```
   host    all    all    <pod-cidr>    scram-sha-256
   ```

Restart PostgreSQL after each change.

!!! danger "Do not use `trust`, and prefer `scram-sha-256` over `md5`"

    Older CyVerse deployment notes used `md5`. New deployments should use
    `scram-sha-256` throughout: it is the default from PostgreSQL 14 onward, and
    `md5` is deprecated upstream.

# Databases and their owners

Created in [phase 2](../02-databases/index.md), listed here because they all
live in this instance:

| Database | Owner role |
|----------|------------|
| iCAT (name chosen at iRODS install time) | `irods` |
| `de` | `de` |
| `notifications` | `de` |
| `metadata` | `de` |
| `qms` | `de` |
| `unleash` | `unleash` |
| `grouper` | `grouper` |
| `portal` | `portal_db_reader` |
| `keycloak` | `keycloak` |

!!! note "Merged databases"

    The former `permissions` database has been merged into the DE database. A
    deployment created from current playbooks will not have a separate
    `permissions` database, and older configuration that refers to one is stale.

# Related

* [Service databases](../02-databases/index.md)
* [Database migrations](../02-databases/migrations.md)
* [iRODS provider](../03-data-store/irods-provider.md)
* [Deploying from scratch](../from-scratch.md#12-postgresql)

[^irods-install]: iRODS 4.3.3 installation guide
