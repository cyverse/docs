
Data Storage in CyVerse is managed both internally and through federation.

??? info "What is Federation?"

    Data federation allows institutions to manage their own hardware, or buy space on commercial cloud, and integrate it with our Data Store.

    Data that are federated do not appear any differently than data that are hosted in the primary CyVerse Data Store.

    _This is not federation in the iRODS sense, where the the federated storage is managed in a separate zone. The storage managed by the institution is in the same zone, `iplant`, as the CyVerse managed storage._

[ds]: ../assets/ds/ds_components.svg
![][ds]{target=_blank}

**Figure**: Access to the Data Store can be accomplished over  HTTPS (`https://`), SFTP (`sftp://`) or iRODS Protocol

## :material-file-tree: iRODS

CyVerse uses iRODS (integrated Rule Oriented Data System) to manage its users data and uses PostgreSQL to manage the catalog (ICAT) database for iRODS.

* [:material-file-tree: iRODS Documentation](https://irods.org/documentation/)
* [:simple-postgresql: PostgreSQL Documentation](https://www.postgresql.org)

## :material-file-arrow-up-down: SFTP

CyVerse uses SFTPGo along with a custom iRODS storage backend to provide SFTP access to data in the Data Store.

* [:material-file-arrow-up-down: SFTPGo Documentation](https://github.com/drakkan/sftpgo)
* [:material-file-arrow-up-down: SFTGO with iRODS Storage Backend Documentation](https://github.com/cyverse/sftpgo)

## :material-file-arrow-up-down: WebDAV

CyVerse uses apache along with the davrods module to manage the WebDAV service for accessing the Data Store over the HTTPS and WebDAVS protocols. Varnish HTTP Cache is used internally for caching requested files.

* [:simple-apache: Apache Documentation](https://httpd.apache.org/docs/)
* [:simple-apache: Davrods Documentation](https://github.com/UtrechtUniversity/davrods)
* [:octicons-cache-16: Varnish HTTP Cache Documentation](https://varnish-cache.org/docs/index.html)

## :material-arrow-decision: HAProxy

CyVerse uses HAProxy to provide a common point of entry, data.cyverse.org, for clients to access all of the Data Store services.

* [:material-arrow-decision: HAProxy Documentation](http://cbonte.github.io/haproxy-dconv/)

## :simple-rabbitmq: AMQP

CyVerse uses RabbitMQ to provide an AMQP-based message bus for notifying other CyVerse services like DataWatch about data related events.

* [:simple-rabbitmq: RabbitMQ Documentation](https://www.rabbitmq.com/documentation.html)
