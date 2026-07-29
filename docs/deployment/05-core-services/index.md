# Phase 5: core services

The services the Discovery Environment authenticates, searches, caches, and
communicates through. Deploy the directory before Keycloak, and Keycloak before
anything that needs its client secrets.

# Identity

* [OpenLDAP](openldap.md) - accounts and POSIX groups
* [Keycloak](keycloak.md) - realm, LDAP federation, mappers, roles, and OAuth clients
* [Grouper](grouper.md) - group management the DE authorizes against

# Search

* [OpenSearch](opensearch.md) - the data search index used by new deployments
* [Elasticsearch (legacy)](elasticsearch.md) - the superseded search stack

# Messaging and state

* [NATS](nats.md) - internal service-to-service messaging
* [Redis HA](redis-ha.md) - caching and session state
* [Unleash](unleash.md) - feature flags

# Storage and utilities

* [iRODS CSI driver](irods-csi-driver.md) - mounting Data Store paths into pods
* [Mail](mail.md) - outbound mail for DE and portal notifications
* [Jaeger](jaeger.md) - distributed tracing (optional)

# Next

* [Phase 6: applications](../06-applications/)
