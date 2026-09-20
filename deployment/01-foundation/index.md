# Phase 1: foundation

Host services installed before anything else. Everything in later phases depends on
at least one of them.

* [HAProxy](https://docs.cyverse.org/deployment/01-foundation/haproxy/) - public entry point; installed now, configured in phase 4
* [PostgreSQL](https://docs.cyverse.org/deployment/01-foundation/postgresql/) - the instance that backs the iRODS catalog and every service database
* [RabbitMQ](https://docs.cyverse.org/deployment/01-foundation/rabbitmq/) - the AMQP bus between iRODS and the DE

# Next

* [Phase 2: databases](https://docs.cyverse.org/deployment/02-databases)
