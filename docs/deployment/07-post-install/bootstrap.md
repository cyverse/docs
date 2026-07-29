---
type: Playbook
title: "Bootstrap"
description: "Creating the first administrator, registering a VICE operator, and importing a starting set of apps."
tags: [deployment, post-install, bootstrap, vice, apps]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: uv
    resource: https://docs.astral.sh/uv/
    title: uv, the Python package and project manager
    author: team:astral
---

# Where this fits

At this point every service is deployed but the deployment has no users, no
registered VICE operator, and no apps. Three steps make it usable.

# Bootstrap portal administrator

```bash
ansible-playbook -i /path/to/inventory bootstrap_portal_admin.yml
```

This creates the `<SITE>-bootstrap` account — the administrator you use for every
step below.

!!! warning "Run this from the right host"

    The playbook does two things that constrain where it can run: it
    `kubectl port-forward`s to OpenLDAP and portal-conductor, and it connects to
    the portal database directly with `psql`. So the host needs cluster access
    **and** a `pg_hba.conf` rule of its own.

    Getting this far means the port-forwarding half already works. The database
    half is the one that fails — see
    [PostgreSQL access control](../01-foundation/postgresql.md#access-control).

Sign in at `https://de.<BASE_DOMAIN>` as `<SITE>-bootstrap` and confirm the
account has the admin navigation before continuing.

# Register a VICE operator

Interactive analyses do not launch until an operator exists. This is a one-time
registration through the DE UI.

1. Sign in to `https://de.<BASE_DOMAIN>` as `<SITE>-bootstrap`.
2. Open the left navigation from the hamburger menu in the upper left, then
   **Admin → VICE**, and select the **Operators** tab.
3. Click **+New** and set:

   | Field | Value |
   |-------|-------|
   | Name | `prod` |
   | URL | `http://vice-operator.vice-apps:10000` |
   | Public base URL | `https://vice.<BASE_DOMAIN>` |
   | Priority | `0` |

4. Click **Register operator**.

The operator URL is plain `http` on purpose: it is an in-cluster service address
that never leaves the cluster network. The **public** base URL is the HTTPS
hostname users' browsers connect to.

# Import a starting set of apps

`scripts/appei` in the deployment repository exports and imports apps and tools
between DE deployments, and manages its own dependencies with
[uv](https://docs.astral.sh/uv/).[^uv] From that directory:

```bash
uv run appei login  --server de.<BASE_DOMAIN> --username <SITE>-bootstrap
uv run appei import --server de.<BASE_DOMAIN> -i de-word-count.json --publish
uv run appei import --server de.<BASE_DOMAIN> -i cloudshell.json --featured
uv run appei import --server de.<BASE_DOMAIN> -i portal-delete-user.json
```

The visibility each flag produces:

| App | Flag | Result |
|-----|------|--------|
| DE Word Count | `--publish` | Public, not featured |
| Cloud Shell | `--featured` | Featured, and therefore public |
| portal-delete-user | none | Private to `<SITE>-bootstrap` |

`--featured` implies public. There is no way to feature a private app, and
attempting it is usually a sign the wrong flag was chosen.

## portal-delete-user

This administrative app needs a configuration file attached before it will run.
Generate the file from your group variables:

```bash
ansible-playbook -i /path/to/inventory portal_delete_user_config.yml
```

That writes `portal-delete-user.json` into the `ansible` directory. Upload it to
the bootstrap user's home directory in the Data Store, keeping the filename.

!!! danger "Delete the local copy afterwards"

    The generated file is populated from the group variables and contains
    credentials. Remove it from your working copy as soon as it is uploaded — it
    sits in a git working tree and is easy to commit by accident.

Then attach it in the DE:

1. **Apps** listing → select **Apps Under Development** from the dropdown.
2. On the `portal-delete-user` row, open the three-dot menu → **Edit App**.
3. Go to the **Parameters** page of the Edit App wizard.
4. Edit the greyed-out **Config File** parameter (the pencil icon on the right).
5. Near the top of the page that opens is another **Config File** box. If it
   holds a value, clear it with the **X**.
6. Click **Browse**, select the uploaded file, and confirm with **OK**.
7. Click **Done**, then **Save** in the top right of the app editor.

Step 7 is not optional — leaving the editor without saving discards the
attachment silently.

If the app or tool imported with the wrong visibility, see
[troubleshooting](./troubleshooting.md#repairing-the-portal-delete-user-import).

# Next

[Verification](./verification.md) — confirm the deployment end to end.

[^uv]: uv, the Python package and project manager
