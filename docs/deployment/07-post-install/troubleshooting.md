---
type: Playbook
title: "Troubleshooting a new deployment"
description: "Fixes for the problems that show up during and just after a first CyVerse deployment."
tags: [deployment, post-install, troubleshooting]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
---

# Migrations were skipped

**Symptom.** DE services start but fail on queries against columns or tables that
should exist.

**Cause.** The DE database expects `standard_conforming_strings = off`. When it is
left on, some migrations do not apply.

**Fix.** Correct the setting in `postgresql.conf`
([PostgreSQL tuning](../01-foundation/postgresql.md#tuning)), restart PostgreSQL,
then re-run the migrations:

```bash
ansible-playbook -i /path/to/inventory --tags=update-databases kubernetes.yml
```

# OpenLDAP is missing the community group

**Symptom.** Group-based entitlements do not resolve for community data, or a
playbook expects a group that is not in the directory.

**Fix.** The deployment repository has an idempotent playbook for it:

```bash
ansible-playbook -i /path/to/inventory openldap_community_group.yml
```

Safe to run when you are not sure whether you need it.

# NATS will not reinstall

**Symptom.** Reinstalling NATS fails because resources already exist.

**Cause.** Helm still holds a release record for the previous install.

**Fix.**

```bash
helm -n prod uninstall nats
```

Then re-run the tag that installs it; see [NATS](../05-core-services/nats.md).

# iRODS logs nothing

**Symptom.** `/var/log/irods/irods.log` is missing or empty even though iRODS is
serving requests.

**Cause.** The rsyslog snippet is in the wrong directory. rsyslog includes
`/etc/rsyslog.d/*.conf`; a file in `/etc/rsyslog/` is silently ignored.

**Fix.** Move the file to `/etc/rsyslog.d/00-irods.conf` and restart rsyslog. See
[iRODS provider](../03-data-store/irods-provider.md#configure-logging-first).

# k0sctl cannot write the kubeconfig

**Symptom.** `k0sctl apply` fails writing the kubeconfig, or a directory named
`config` appears where the file should be.

**Cause.** Creating the *basename* of `$KUBECONFIG` instead of its *dirname*.

**Fix.**

```bash
mkdir -p "$(dirname "$KUBECONFIG")"
```

# Pods cannot reach PostgreSQL

**Symptom.** Services in the cluster fail to connect to the database while `psql`
from an admin host works.

**Cause.** The pod CIDR is not in `pg_hba.conf`. It cannot be added before the
cluster exists, so it is easy to skip.

**Fix.** [Add the pod CIDR](../01-foundation/postgresql.md#access-control) and
restart PostgreSQL.

# ImagePullBackOff on an image that exists

**Cause.** A missing or wrong registry pull secret in the namespace, rather than a
registry problem — `harbor-registry-credentials` for DE services,
`vice-image-pull-secret` for interactive apps.

**Fix.** See [Harbor](../04-kubernetes/harbor.md) and
[VICE](../06-applications/vice.md).

# Repairing the portal-delete-user import

**Symptom.** The `portal-delete-user` app or tool imported with the wrong
visibility, or has to be re-imported cleanly.

The app and tool are deleted **by ID**, and the simplest way to learn both IDs is
to re-run the import: it is a no-op when they already exist, and it prints the
IDs. From `scripts/appei` in the deployment repository:

```bash
uv run appei login  --server de.<BASE_DOMAIN> --username <SITE>-bootstrap

# no-op re-import; prints the tool and app IDs
uv run appei import --server de.<BASE_DOMAIN> -i portal-delete-user.json

# hard delete the app, then the tool
uv run appei shred-app    --server de.<BASE_DOMAIN> --id <APP_ID>
uv run appei delete-tool  --server de.<BASE_DOMAIN> --id <TOOL_ID>

# re-import, private this time
uv run appei import --server de.<BASE_DOMAIN> -i portal-delete-user.json
```

Both should now be visible to `<SITE>-bootstrap` and private to that account.

`shred-app` is a permanent hard delete, not a soft delete. Confirm the app ID
before running it.

Then re-attach the configuration file:
[portal-delete-user](./bootstrap.md#portal-delete-user).

# Related

* [Verification](./verification.md)
* [FAQ](../../operations/faq.md)
* [Deploying from scratch](../from-scratch.md)
