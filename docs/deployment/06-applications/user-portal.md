---
type: Deployment Procedure
title: "User Portal"
description: "Deploying the User Portal and creating the LDAP, iRODS, and database accounts it needs."
tags: [deployment, applications, user-portal, accounts]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: portal2
    resource: https://gitlab.com/cyverse/portal2
    title: CyVerse User Portal (portal2)
    author: team:cyverse
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
---

# Role in the deployment

The User Portal is where accounts are created and access to services is
requested and granted. It writes to three systems, which is why it needs an
identity in each of them:

| System | Identity | Why |
|--------|----------|-----|
| LDAP | `uid=portal,ou=People,<LDAP_BASE_DN>` | Creates and updates user entries |
| iRODS | `portal` (`rodsadmin`) | Creates home collections for new accounts |
| PostgreSQL | `portal_db_reader` | Owns the portal database |

# Prerequisites

* [OpenLDAP](../05-core-services/openldap.md) running, with the `portal` service
  account created and added to `de_admins`.
* [Portal database](../02-databases/portal.md) created, restored, and seeded.
* [Keycloak](../05-core-services/keycloak.md) configured with the
  `portal-<SITE>` client, and its ID and secret in the group variables.

# iRODS account

The portal creates users' home collections, so it needs an iRODS admin account of
its own — not the DE's:

```bash
# as the irods service account
iadmin mkuser portal rodsadmin
iadmin moduser portal password '<GENERATED_SECRET>'
```

# Images

The portal deployment pulls two images:

| Image | Notes |
|-------|-------|
| `nginx:1.20-alpine` | Static front end; pull through your own registry rather than Docker Hub to avoid rate limits |
| `<REGISTRY>/portal:<TAG>` | The portal application itself |

Build the portal image from
[portal2](https://gitlab.com/cyverse/portal2)[^portal2] and push it to your own
[Harbor](../04-kubernetes/harbor.md) project. Older notes reference a personal
Docker Hub image; do not deploy from one — an image nobody at your site controls
is an unpinned dependency in your authentication path.

# Deploy

```bash
kubectl create ns user-portal
kubectl apply -k portal/user-portal/base -n user-portal
```

The portal is also deployed as part of the `deploy-all-services` tag; deploy it
by hand only when you are iterating on the portal specifically.

# Verify

1. `https://user.<BASE_DOMAIN>` loads.
2. Sign-in redirects to Keycloak and back.
3. A test account request appears in the admin panel.

Account creation exercises all three identities above. A request that is accepted
but never produces a usable account is normally the LDAP or iRODS credential, not
the portal.

# Next

* [Bootstrap the first administrator](../07-post-install/bootstrap.md)
* [User Portal administration](../../operations/user-portal.md)

[^portal2]: CyVerse User Portal (portal2)
