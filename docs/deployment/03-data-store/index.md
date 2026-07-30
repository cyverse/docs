# Phase 3: Data Store

The iRODS zone that holds user data. It needs PostgreSQL and RabbitMQ from
[phase 1](../01-foundation/), and everything in later phases reads or writes
through it.

* [iRODS catalog provider](irods-provider.md) - install iRODS 4.3.3, apply CyVerse policy, initialize the zone
* [iRODS integration for the DE](de-integration.md) - specific queries, the DE service account, and the event flow

# Related

* [iCAT database](../02-databases/icat.md)
* [iRODS CSI driver](../05-core-services/irods-csi-driver.md) - mounting collections into pods
* [Data Store](../../platform/data-store.md) - the access services on top of the zone

# Next

* [Phase 4: Kubernetes](../04-kubernetes/)
