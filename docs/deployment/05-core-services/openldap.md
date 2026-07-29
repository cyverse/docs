---
type: Deployment Procedure
title: "OpenLDAP"
description: "Deploying the LDAP directory that holds CyVerse accounts and groups, and the service accounts that read it."
tags: [deployment, core-services, ldap, openldap, accounts]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
---

# Role in the deployment

OpenLDAP is the system of record for CyVerse accounts and POSIX groups. Keycloak
federates it read-only, the User Portal creates entries in it, and the DE reads
group membership from it through Grouper. Deploy it before
[Keycloak](./keycloak.md) — Keycloak's user federation configuration fails
without something to bind to.

# Install

```bash
ansible-playbook -i /path/to/inventory --tags de-reqs         kubernetes.yml
ansible-playbook -i /path/to/inventory --tags openldap-docker kubernetes.yml
```

The `de-reqs` tag installs shared prerequisites and runs first.

In-cluster, the directory is reachable as `ldap://openldap.openldap` — service
`openldap` in namespace `openldap`. That is the URL Keycloak's federation
provider uses; there is no need to expose LDAP outside the cluster.

# Directory layout

| Branch | Holds |
|--------|-------|
| `ou=People,<LDAP_BASE_DN>` | User accounts (`inetOrgPerson`, `posixAccount`) |
| `ou=Groups,<LDAP_BASE_DN>` | POSIX groups (`posixGroup`), including `de_admins` |

The group branch is what Keycloak's `entitlement` and `roles` mappers read; see
[Keycloak](./keycloak.md#ldap-mappers).

# Service accounts

Two accounts matter to the rest of the deployment:

| Account | Used by | Variable |
|---------|---------|----------|
| LDAP reader | Keycloak federation bind | `ldap_cn`, `ldap_base_dn`, `ldap_ldap_reader_pw` |
| `portal` | User Portal account creation | portal group variables |

The reader account only needs read access; give it nothing more. Its bind DN is
assembled from the inventory variables, for example
`uid=ldap_reader,ou=People,<LDAP_BASE_DN>`.

## Creating the portal service account

The User Portal needs an LDAP identity to create accounts with. Create it from an
LDIF, then set its password separately so the password never appears in a file:

```ldif
dn: uid=portal,ou=People,<LDAP_BASE_DN>
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
uid: portal
mail: portal@<BASE_DOMAIN>
sn: ServiceAccount
givenName: PORTAL
cn: portal
title: Other
o: N/A
departmentNumber: N/A
uidNumber: 40003
gidNumber: 10003
homeDirectory: /home/portal
```

```bash
ldapadd -x -D "cn=Manager,<LDAP_BASE_DN>" -W -f portal-user.ldif
ldappasswd -x -D "cn=Manager,<LDAP_BASE_DN>" -W -S "uid=portal,ou=People,<LDAP_BASE_DN>"
```

`-W` and `-S` prompt for the passwords instead of taking them on the command
line, where they would land in shell history and process listings.

Then add the account to the DE administrators group:

```ldif
dn: cn=de_admins,ou=Groups,<LDAP_BASE_DN>
changetype: modify
add: memberUid
memberUid: portal
```

```bash
ldapmodify -x -D "cn=Manager,<LDAP_BASE_DN>" -W -f portal-de_admin.ldif
```

See [User Portal](../06-applications/user-portal.md) for the iRODS and database
accounts the portal also needs.

# Adding the community group later

If an existing deployment is missing the community group, the deployment
repository has an idempotent playbook for it:

```bash
ansible-playbook -i /path/to/inventory openldap_community_group.yml
```

It is safe to run when you are not sure whether it is needed.

# Related

* [Keycloak](./keycloak.md)
* [Grouper](./grouper.md)
* [User Portal](../06-applications/user-portal.md)
