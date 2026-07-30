---
type: Deployment Procedure
title: "Keycloak"
description: "Deploying Keycloak and configuring the realm, LDAP federation, mappers, roles, and OAuth clients the DE requires."
tags: [deployment, core-services, keycloak, authentication, oauth]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: keycloak-docs
    resource: https://www.keycloak.org/documentation
    title: Keycloak documentation
    author: team:keycloak
---

# Prerequisites

* [Keycloak database](../02-databases/keycloak.md) created.
* [OpenLDAP](./openldap.md) running, with the reader service account and the
  `ou=People` and `ou=Groups` branches ready.
* [cert-manager](../04-kubernetes/cert-manager.md) and
  [ingress](../04-kubernetes/ingress.md) in place, so
  `keycloak.<BASE_DOMAIN>` can be served over HTTPS.

# Deploy

```bash
ansible-playbook -i /path/to/inventory --tags keycloak kubernetes.yml
```

Add DNS for `keycloak.<BASE_DOMAIN>` — a `CNAME` to the ingress is typical. An
`/etc/hosts` entry will carry you through the configuration below, but it is not
a deployment; fix DNS before anything else points at Keycloak.

??? info "Deploying by kustomize instead of the playbook"

    Older deployments applied Keycloak with kustomize, generating its secrets and
    configuration inline. The shape of that configuration is still a useful
    reference for what Keycloak needs:

    ```yaml
    secretGenerator:
    - name: dbuser
      literals:
      - username=<USERNAME>
      - password=<GENERATED_SECRET>
    - name: kcadmin
      literals:
      - username=<USERNAME>
      - password=<GENERATED_SECRET>
    configMapGenerator:
    - name: keycloak-config
      literals:
      - KEYCLOAK_HOSTNAME=keycloak.<BASE_DOMAIN>
      - KEYCLOAK_LOGLEVEL=INFO
      - DB_VENDOR=postgres
      - DB_ADDR=<DATABASE_HOST>
      - DB_PORT=5432
      - PROXY_ADDRESS_FORWARDING=true
      - JDBC_PARAMS=connectTimeout=21600
    namespace: keycloak
    resources:
    - deployment.yaml
    - service.yaml
    generatorOptions:
      disableNameSuffixHash: true
    ```

    ```bash
    kubectl apply -k ./base/ -n keycloak
    ```

    Generate every literal at install time. Never commit a rendered
    `kustomization.yaml` containing real secrets.

# Realm

Create a realm named for your site (`<SITE>`). It can take a few moments to
appear; confirm it is selected as the current realm before continuing — the rest
of this configuration silently lands in `master` otherwise.

**Configure → Realm settings**:

| Setting | Value |
|---------|-------|
| Realm name | `<SITE>` |
| Display name / HTML display name | Whatever users should see |
| Frontend URL | blank |
| Require SSL | All requests |

Leave everything else at its default, and save at the bottom of the page.

# LDAP user federation

**Configure → User federation → Add LDAP provider**, with the realm selected:

| Setting | Value |
|---------|-------|
| Vendor | Other |
| Connection URL | `ldap://openldap.openldap` |
| StartTLS | off |
| Use Truststore SPI | Always |
| Connection pooling | on |
| Connection timeout | blank |
| Bind type | simple |
| Bind DN | `uid=ldap_reader,ou=People,<LDAP_BASE_DN>` (from `ldap_cn` and `ldap_base_dn`) |
| Bind credentials | the value of `ldap_ldap_reader_pw` |

Use **Test connection** and **Test authentication** before going further. A toast
in the upper right reports the result; if either fails, nothing below will work.

LDAP searching and updating:

| Setting | Value |
|---------|-------|
| Edit mode | `READ_ONLY` |
| Users DN | the bind DN without the `uid` component, e.g. `ou=People,<LDAP_BASE_DN>` |
| Relative user creation | blank |
| Username LDAP attribute | `uid` |
| RDN LDAP attribute | `uid` |
| UUID LDAP attribute | `uidNumber` |
| User object classes | `inetOrgPerson, posixAccount` |
| LDAP filter | blank |
| Search scope | One Level |
| Read timeout | blank |
| Pagination | on |
| Referral | blank |

Synchronization:

| Setting | Value |
|---------|-------|
| Import users | on |
| Sync registrations | off |
| Batch size | `500` |
| Periodic full sync | on, period `86400` |
| Periodic changed users sync | on, period `3600` |
| Remove invalid users during searches | on |

Leave Kerberos integration off, cache settings at `DEFAULT`, and advanced
settings off. Save.

!!! note "`READ_ONLY` and sync registrations off are deliberate"

    Accounts are created in LDAP by the User Portal, not in Keycloak. Letting
    Keycloak write back would produce two systems of record for one account.

# LDAP mappers

**Configure → User federation → (the LDAP provider) → Mappers → Add mapper.**
Three mappers are needed.

## entitlement

| Setting | Value |
|---------|-------|
| Name | `entitlement` |
| Mapper type | `group-ldap-mapper` |
| LDAP Groups DN | `ou=Groups,<LDAP_BASE_DN>` |
| Group Object Classes | `posixGroup` |
| Preserve Group Inheritance | off |
| Membership LDAP Attribute | `memberUid` |
| Membership Attribute Type | `UID` |
| Membership User LDAP Attribute | `uid` |
| Mode | `READ_ONLY` |
| User Groups Retrieve Strategy | `LOAD_GROUPS_BY_MEMBER_ATTRIBUTE` |
| Member-Of LDAP Attribute | `memberOf` |
| Drop non-existing groups during sync | on |
| Groups Path | `/` |

Preserve Group Inheritance must be off because POSIX groups are flat; leaving it
on makes the sync fail on groups that have no parent.

## name

| Setting | Value |
|---------|-------|
| Name | `name` |
| Mapper type | `user-attribute-ldap-mapper` |
| User Model Attribute | `name` (typed into the text box, nothing selected from the dropdown) |
| LDAP attribute | `cn` |
| Read Only | on |
| Always Read Value From LDAP | on |
| Is Mandatory in LDAP | on |
| Force a Default Value | on |

## roles

| Setting | Value |
|---------|-------|
| Name | `roles` |
| Mapper type | `role-ldap-mapper` |
| LDAP Roles DN | `ou=Groups,<LDAP_BASE_DN>` (same as the entitlement mapper) |
| Role Object Classes | `posixGroup` |
| Membership LDAP Attribute | `memberUid` |
| Membership Attribute Type | `UID` |
| Mode | `READ_ONLY` |

Everything not listed can stay at its default.

# Client scope mappers

The DE reads `name` and `entitlement` out of the token, so both have to be added
to the `profile` client scope. **Manage → Client scopes → profile → Mappers →
Add mapper → By configuration**:

| Mapper | Type | Settings |
|--------|------|----------|
| `name` | User Property | Name, Property, and Token Claim Name all set to `name` |
| `entitlement` | Group Membership | Name and Token Claim Name set to `entitlement`; **Full group path off** |

Leaving Full group path on prefixes every group with `/`, and the DE's
entitlement checks then match nothing.

# Authentication

**Configure → Authentication**. Leave the Flows tab alone. In **Required
actions**, enable exactly these and disable the rest:

* Terms and Conditions
* Update Password
* Update Profile
* Verify Email
* Delete Credential
* Linking Identity Provider
* Update User Locale

# Realm roles

**Manage → Realm roles**, add:

| Role | Held by |
|------|---------|
| `app-runner` | Accounts permitted to run apps |
| `cyverse-emailer` | The service that sends mail |
| `cyverse-ldap-reader` | The directory reader |
| `cyverse-subscription-updater` | Subscription and quota updates |
| `vice-operator` | The VICE operator service account |

# Clients

Eight clients. **Manage → Clients → Create client.** The create wizard presents
fields in a different order than the finished client page, so work from the
tables below rather than from wizard step order, and re-check the client detail
page when you are done.

Every client secret below is read from the **Credentials** tab on the client
detail page and written into `group_vars/all.yml` in your **private** inventory.

## Interactive clients

| Client ID | Root / Home URL | Valid redirect URIs | Web origins | Admin URL |
|-----------|-----------------|---------------------|-------------|-----------|
| `de-<SITE>` | `https://de.<BASE_DOMAIN>` | `https://de.<BASE_DOMAIN>/*` | `https://de.<BASE_DOMAIN>` | `https://de.<BASE_DOMAIN>` |
| `portal-<SITE>` | `https://user.<BASE_DOMAIN>` | `https://user.<BASE_DOMAIN>/*` | `https://user.<BASE_DOMAIN>` | `https://user.<BASE_DOMAIN>` |
| `vice-<SITE>` | — | `https://*` | `https://*.vice.<BASE_DOMAIN>/*` | — |

All three: client authentication **on**, authorization **off**, valid post logout
redirect URIs `+`. Authentication flow: Standard flow and Direct access grants
checked and nothing else — except `vice-<SITE>`, which also needs Service account
roles.

| Client | Client ID variable | Secret variable |
|--------|--------------------|-----------------|
| `de-<SITE>` | `keycloak_client_id` | `keycloak_client_secret` |
| `vice-<SITE>` | `keycloak_vice_client_id` | `keycloak_vice_client_secret` |
| `portal-<SITE>` | `portal_keycloak_client` | `portal_keycloak_secret` |

## VICE service clients

| Client ID | Purpose | Configuration |
|-----------|---------|---------------|
| `vice-api` | Lets the app-exposer backend reach the VICE operator | Client authentication on; **only** Service account roles checked. After saving: **Service account roles → Assign role → Realm roles → `vice-operator` → Assign** |
| `vice-users` | VICE authentication callback | Client authentication on; Standard flow and Direct access grants checked; valid redirect URI `https://vice-api.vice.<BASE_DOMAIN>/auth/callback`; web origins `+` |
| `vice-swagger` | VICE API documentation UI | Client authentication on, authorization off; Standard flow checked; valid redirect URI `https://vice-api.vice.<BASE_DOMAIN>/docs/callback`; web origins `https://vice-api.vice.<BASE_DOMAIN>`; post logout redirect `+` |

Their client IDs default to the names above, so only the secrets need setting:

| Client | Secret variable |
|--------|-----------------|
| `vice-api` | `vice_api_keycloak_client_secret` |
| `vice-users` | `vice_operator_keycloak_client_secret` |
| `vice-swagger` | `vice_operator_swagger_client_secret` |

!!! note "Two variable names do not match their client names"

    `vice-users` and `vice-swagger` write into variables named for the *operator*.
    That is what the playbooks read, so copy the values into the variables exactly
    as listed rather than "correcting" them.

## Remaining service clients

| Client ID | Purpose | Configuration | Variables |
|-----------|---------|---------------|-----------|
| `formation-service-account` | Service-to-service access for formation | Client authentication on; Service account roles checked; everything else default | `formation_keycloak_client_id`, `formation_keycloak_client_secret` |
| `de-admin-api` | DE administrative API | Root and Home URL `https://de.<BASE_DOMAIN>`; redirect `https://de.<BASE_DOMAIN>/*`; post logout `+`; web origins `+`; Admin URL `https://de.<BASE_DOMAIN>`; client authentication on; Standard flow **and** Service account roles checked, **Direct access grants unchecked** | `keycloak_admin_client_id`, `keycloak_admin_client_secret` |

# After Keycloak

The client secrets you just collected are inputs to the phase that follows, so
finish this document before running further playbooks. Next:

1. [Generate service signing keys](../from-scratch.md#53-service-signing-keys).
2. Apply configuration, ingress, networking, and NATS.

# Related

* [Authentication architecture](../../platform/authentication.md)
* [OpenLDAP](./openldap.md)
* [Keycloak database](../02-databases/keycloak.md)
