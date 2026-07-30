---
type: Deployment Procedure
title: "iRODS catalog provider"
description: "Installing iRODS 4.3.3 as a catalog provider, applying CyVerse policy, and initializing the zone."
tags: [deployment, data-store, irods, storage]
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
  - id: irods-packages
    resource: https://packages.irods.org/
    title: iRODS package repository setup
    author: team:irods-consortium
  - id: ds-collection
    resource: https://github.com/cyverse/ds-collection
    title: CyVerse Data Store collection (playbooks and iRODS policy)
    author: team:cyverse-devops
---

# Prerequisites

* [PostgreSQL](../01-foundation/postgresql.md) installed, tuned, and prepared
  for iRODS.
* [RabbitMQ](../01-foundation/rabbitmq.md) running, with the `/data-store`
  vhost, the `irods` topic exchange, and an iRODS account.
* The vault filesystem mounted, with enough capacity for the data the zone will
  hold.

The Data Store collection's `playbooks/irods_catalog_provider.yml` automates
most of what follows. The manual steps are documented because the playbooks
target older iRODS releases and need review against 4.3.3.

# Configure logging first

iRODS logs through syslog, so set this up **before** the first server start or
the initial run is lost.

`/etc/rsyslog.d/00-irods.conf`:

```
$FileCreateMode 0644
$DirCreateMode 0755
$Umask 0000
$template irods_format,"%msg%\n"
:programname,startswith,"irodsServer" /var/log/irods/irods.log;irods_format
& stop
:programname,startswith,"irodsDelayServer" /var/log/irods/irods.log;irods_format
& stop
:programname,startswith,"irodsAgent" /var/log/irods/irods.log;irods_format
& stop
```

!!! note "Path correction"

    Earlier deployment notes give this path as `/etc/rsyslog/00-irods.conf`.
    rsyslog includes `/etc/rsyslog.d/*.conf`, so a file placed in
    `/etc/rsyslog/` is silently ignored and iRODS logging appears not to work.

`/etc/logrotate.d/irods`:

```
/var/log/irods/irods.log {
        weekly
        rotate 26
        copytruncate
        delaycompress
        compress
        dateext
        notifempty
        missingok
        su root root
}
```

Twenty-six weekly rotations keeps six months of history. `copytruncate` is used
because the iRODS server holds the log open.

# Install

1. Set TCP keepalive to 120 seconds with `sysctl`, so long-lived agent
   connections are not dropped by intermediate firewalls.
2. Install `python-is-python3` and `python3-pika` — the CyVerse policy scripts
   are Python 3 and publish to AMQP.
3. Add the iRODS apt repository.[^irods-packages]
4. **Pin `irods-*` to `4.3.3`.** Without a pin, an unattended upgrade can move
   the catalog provider to a release the policy has not been tested against.
5. Run the iRODS setup script.[^irods-install]

## Setup answers

| Prompt | Value |
|--------|-------|
| iRODS user / group | `irods` / `irods` |
| Server role | `provider` |
| ODBC driver for PostgreSQL | `PostgreSQL Unicode` |
| Catalog host / port | `localhost` / `5432` |
| Catalog database name | your iCAT database name |
| Catalog database user | `irods` |
| Password salt | a generated alphanumeric string — see the warning below |
| Local storage on this server | `yes` |
| Default resource name | anything except `demoResc` |
| Vault directory | root of the filesystem that will hold the data |
| Zone name | `<IRODS_ZONE>`, something meaningful to the project or institution |
| Server port | `1247` |
| Port range | `20000` to `20199` |
| Control plane port | `1248` |
| Schema validation base URI | `file:///var/lib/irods/configuration_schemas` |
| Administrator username | `rods` |
| Zone key | generated alphanumeric string, fewer than 40 characters |
| Negotiation key | generated alphanumeric string |

!!! danger "Never accept an empty password salt"

    An empty salt makes the passwords stored in the catalog recoverable.
    Generate one per install (`openssl rand -hex 16`) and keep it with the rest
    of the deployment secrets in your private inventory.

!!! warning "The zone name is effectively permanent"

    Zone names appear in every published data URL. The production US zone is
    still named `iplant` after a project that was renamed years ago, precisely
    because renaming would break every published link. Choose a name you can
    live with.

# Install CyVerse policy

From the [Data Store collection](https://github.com/cyverse/ds-collection)
branch that matches your site,[^ds-collection] as the `irods` service account:

1. Copy `playbooks/files/irods/var/lib/irods/msiExecCmd_bin/*` into
   `/var/lib/irods/msiExecCmd_bin/` and make them executable.
2. Render `playbooks/templates/irods/etc/irods/cyverse-env.re.j2` to
   `/etc/irods/cyverse-env.re`:
   * `cyverse_RE_HOST` — the FQDN of the catalog provider host.
   * `cyverse_ZONE` — `<IRODS_ZONE>`.
3. Copy `playbooks/files/etc/irods/*` into `/etc/irods/`.

Then set in `/etc/irods/server_config.json`:

| Key | Value |
|-----|-------|
| `advanced_settings.number_of_concurrent_delay_rule_executors` | `12` |
| `environment_variables.IRODS_AMQP_URI` | `amqp://<USER>:<GENERATED_SECRET>@localhost:5672/%2Fdata-store` |
| `plugin_configuration.rule_engines[0].re_rulebase_set` | `["cve", "cyverse_core", "core"]` |

The rule base order is significant: `cve` overrides `cyverse_core`, which
overrides `core`. Reordering them silently changes policy.

`%2F` in the AMQP URI is the URL-encoded leading slash of the `/data-store`
vhost — it is not a typo.

Enable the service so it starts at boot, then start it.

# Initialize the zone

As the `irods` service account, substituting `<IRODS_ZONE>` throughout.

## Administrative group

1. Create the `rodsadmin` group and add `rods` to it.
2. Delete the collections group creation leaves behind:
   `/<IRODS_ZONE>/home/rodsadmin` and `/<IRODS_ZONE>/trash/home/rodsadmin`.
3. Delete `/<IRODS_ZONE>/trash/home/public`.

## UUIDs on predefined collections

CyVerse policy expects every collection to carry a time-based (version 1) UUID.
The collections created by the installer predate the policy, so check and add
them for:

`/<IRODS_ZONE>`, `/<IRODS_ZONE>/home`, `/<IRODS_ZONE>/home/public`,
`/<IRODS_ZONE>/home/rods`, `/<IRODS_ZONE>/trash`, `/<IRODS_ZONE>/trash/home`,
`/<IRODS_ZONE>/trash/home/rods`.

## Permissions

| Group or user | Permission | Collections |
|---------------|------------|-------------|
| `rodsadmin` | `write` | `/<IRODS_ZONE>`, `/<IRODS_ZONE>/home`, `/<IRODS_ZONE>/trash`, `/<IRODS_ZONE>/trash/home` |
| `rodsadmin` | `own` | `/<IRODS_ZONE>/home/rods`, `/<IRODS_ZONE>/trash/home/rods` |
| `anonymous` | `read` | `/<IRODS_ZONE>`, `/<IRODS_ZONE>/home` |

## Anonymous access

Create the `anonymous` `rodsuser` **without a password**, then grant it the
`read` permissions above. This account is what makes public data public — WebDAV
and the Data Commons read as `anonymous`. A password on this account breaks
anonymous access rather than securing it.

# Next

* [DE integration](./de-integration.md) — the specific queries and service
  account the Discovery Environment needs.
* [iRODS CSI driver](../05-core-services/irods-csi-driver.md) — mounting Data
  Store paths into VICE pods.
* [Data Store](../../platform/data-store.md) — the access services layered on
  top of the zone.

[^irods-install]: iRODS 4.3.3 installation guide
[^irods-packages]: iRODS package repository setup
[^ds-collection]: CyVerse Data Store collection (playbooks and iRODS policy)
