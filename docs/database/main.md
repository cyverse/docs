# Databases

CyVerse uses [:simple-postgresql: PostgreSQL](https://www.postgresql.org/) as its primary database platform.

Each database is maintained in its own GitHub Repository in the core [:simple-github: CyVerse Organization](https://github.com/cyverse){target=_blank} or [:simple-github: CyVerse Discovery Environment Organization](https://github.com/cyverse-de){target=_blank}

## :octicons-database-24: Provisioning 

[:octicons-database-24: iCAT]() - the iRODS iCAT metadata database

[:octicons-database-24: DE](../database/de-db.md) - the Discovery Environment database

[:octicons-database-24: Metadata](../database/metadata-db.md) - the CyVerse Metadata Service

[:octicons-database-24: Keycloak](../database/keycloak-db.md) - the database used by Keycloak

[:octicons-database-24: Notifications](../database/notifications-db.md) - the database used for notifications

[:octicons-database-24: Unleash](../database/unleash-db.md) - the database used for Unleash service

[:octicons-database-24: Grouper](../database/grouper-db.md) - the database for Grouper service

[:octicons-database-24: QMS](../database/qms-db.md) - the database for QMS service

[:octicons-database-24: Portal](../database/portal-db.md) - the User Portal database

## :material-map-search-outline: User Guides

[:fontawesome-solid-gears: DevOps](../guides/devops.md) - instructions for setting up a remote DevOps environment to manage a CyVerse deployment

[Discovery Environment](../guides/de.md) - designed for data scientists, students, and researchers who want to use CyVerse for research.

[Data Store](../guides/ds.md) - instructions for using the iRODS Data Store.

**This database dedicated to the *discovery environment* of Cyverse, which is used for multiple services such as:**

* [de](de-db.md)
* [notifications](notifications-db.md)
* [metadata](metadata-db.md)
* [unleash](unleash-db.md)
* [grouper](grouper-db.md)
* [qms](qms-db.md)
* [portal](portal-db.md)
* [keycloak](keycloak-db.md)

**NOTE: permissions database has been merged with DE database.**

## Install
The installation of this database is manully done on the host `DB_HOST.com'`.
See documentation on [how to install postgresql?](https://www.postgresguide.com/setup/install/)

**Installing postgresql 12 on Centos7**

```bash
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

sudo yum install -y postgresql12-server
sudo /usr/pgsql-12/bin/postgresql-12-setup initdb
sudo systemctl enable postgresql-12
sudo systemctl start postgresql-12
sudo yum install postgresql12-contrib
```

## Setup
On this paragraph  we will cover first and necessary steps to configure the database.


**~postgres/12/data/pg_hba.conf**

**IPv4 local connections:**

Add IP or IP range of kubernetes worker node, that requires connection to this database.

| TYPE | DATABASE | USER | ADDRESS | METHOD |
|------|----------|------|---------|--------|
|host  |all       | all  | *******/32 | md5   |
|host  |all       | all  | *******/32 | md5   |
|host  |all       | all  | *******/32 | md5   |
|host  |all       | all  | *******/32 | md5   |
|host  |all       | all  | *******/32 | md5   |
|host  |all       | all  | *******/32 | md5   |

**~postgres/12/data/postgresql.conf**

```bash
# vi ~postgres/12/data/postgresql.conf
listen_addresses = '*'          # what IP address(es) to listen on;
```

** .pgpass**

**TODO:**

### Databases and its Users

| DATABASE | USER |
|----------|------|
|  de  |    de    |
|  notifications  |    de    |
|  metadata |  de    |
|  unleash  | unleash_user    |
|  grouper |  grouper    |
|  portal  |  portal    |
|  keycloak |    keycloak |
|  qms |    de |

