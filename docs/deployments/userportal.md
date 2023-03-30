# User Portal

!!! success "Prerequisites"


### TODO: add source location.

### LDAP
Setup required ldap user.

#### create portal user
##### Create portal-user.ldif

```ldif
dn: uid=portal,ou=People,dc=tugraz,dc=at
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
uid: portal
mail: portal@example.com
sn: SeviceAccount
givenName: PORTAL
cn: portal
title: Other
o: N/A
departmentNumber: N/A
uidNumber: 40003
gidNumber: 10003
homeDirectory: /home/portal
```

##### Apply ldap file

```bash
# replace LDAP_PASSWORD 
ldapadd -x -D "cn=Manager,dc=tugraz,dc=at" -w "LDAP_PASSWORD" -f portal-user.ldif

# create a password for the user
## replace PORTAL_PASSWORD & LDAP_PASSWORD
ldappasswd -x -D cn=Manager,dc=tugraz,dc=at -w "LDAP_PASSWORD" -s "PORTAL_PASSWORD" "uid=portal,ou=People,dc=tugraz,dc=at"
```

#### Add portal user to admim group

##### Create portal-de_admin.ldif

```ldif
dn: cn=de_admins,ou=Groups,dc=tugraz,dc=at
changetype: modify
add: memberUid
memberUid: portal
```

##### Apply ldap file

```bash
# replace LDAP_PASSWORD
## add user to the de_admin group
ldapmodify -x -D "cn=Manager,dc=tugraz,dc=at" -w "LDAP_PASSWORD" -f portal-de_admin.ldif
```


### iRODs

For user portal we would need to create an irods `rodsadmin` user.

```bash
# replace YOURPASSWORDHERE 
## su - irods
iadmin mkuser portal rodsadmin
iadmin moduser portal password YOURPASSWORDHERE
```

### Database

For Database configuration and setup vist [portal-db docs](../database/portal-db.md).

### Images

1. harbor.cyverse.org/hub/library/nginx:1.20-alpine **Replaced with**:nginx:1.20-alpine

2. mbwali/portal:tug-stable
    * Todo: tekton should build this via the URL.

## Deploy

```bash
# create ns
kubectl create ns user-portal

# deploy 
kubectl apply -k portal/user-portal/base -n user-portal
```
