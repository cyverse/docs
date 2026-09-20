# Phase 5: core services

The services the Discovery Environment authenticates, searches, caches, and
communicates through. Deploy the directory before Keycloak, and Keycloak before
anything that needs its client secrets.

# Identity

* [OpenLDAP](https://docs.cyverse.org/deployment/05-core-services/openldap/) - accounts and POSIX groups
* [Keycloak](https://docs.cyverse.org/deployment/05-core-services/keycloak/) - realm, LDAP federation, mappers, roles, and OAuth clients
* [Grouper](https://docs.cyverse.org/deployment/05-core-services/grouper/) - group management the DE authorizes against

# Search

* [OpenSearch](https://docs.cyverse.org/deployment/05-core-services/opensearch/) - the data search index used by new deployments
* [Elasticsearch (legacy)](https://docs.cyverse.org/deployment/05-core-services/elasticsearch/) - the superseded search stack

# Messaging and state

* [NATS](https://docs.cyverse.org/deployment/05-core-services/nats/) - internal service-to-service messaging
* [Redis HA](https://docs.cyverse.org/deployment/05-core-services/redis-ha/) - caching and session state
* [Unleash](https://docs.cyverse.org/deployment/05-core-services/unleash/) - feature flags

# Storage and utilities

* [iRODS CSI driver](https://docs.cyverse.org/deployment/05-core-services/irods-csi-driver/) - mounting Data Store paths into pods
* [Mail](https://docs.cyverse.org/deployment/05-core-services/mail/) - outbound mail for DE and portal notifications
* [Jaeger](https://docs.cyverse.org/deployment/05-core-services/jaeger/) - distributed tracing (optional)

# Next

* [Phase 6: applications](https://docs.cyverse.org/deployment/06-applications)
