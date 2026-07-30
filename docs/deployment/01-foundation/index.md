# Phase 1: foundation

Host services installed before anything else. Everything in later phases depends on
at least one of them.

* [HAProxy](haproxy.md) - public entry point; installed now, configured in phase 4
* [PostgreSQL](postgresql.md) - the instance that backs the iRODS catalog and every service database
* [RabbitMQ](rabbitmq.md) - the AMQP bus between iRODS and the DE

# Next

* [Phase 2: databases](../02-databases/)
