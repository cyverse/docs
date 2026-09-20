# Phase 3: Data Store

The iRODS zone that holds user data. It needs PostgreSQL and RabbitMQ from
[phase 1](https://docs.cyverse.org/deployment/01-foundation), and everything in later phases reads or writes
through it.

* [iRODS catalog provider](https://docs.cyverse.org/deployment/03-data-store/irods-provider/) - install iRODS 4.3.3, apply CyVerse policy, initialize the zone
* [iRODS integration for the DE](https://docs.cyverse.org/deployment/03-data-store/de-integration/) - specific queries, the DE service account, and the event flow

# Related

* [iCAT database](https://docs.cyverse.org/deployment/02-databases/icat/)
* [iRODS CSI driver](https://docs.cyverse.org/deployment/05-core-services/irods-csi-driver/) - mounting collections into pods
* [Data Store](https://docs.cyverse.org/platform/data-store/) - the access services on top of the zone

# Next

* [Phase 4: Kubernetes](https://docs.cyverse.org/deployment/04-kubernetes)
