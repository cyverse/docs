
Data Storage in CyVerse is managed both internally and through federation.

??? info "What is Federation?"

    Data federation allows institutions to manage their own hardware, or buy space on commercial cloud, and integrate it with our Data Store.

    Data that are federated do not appear any differently than data that are hosted in the primary CyVerse Data Store. 

[ds]: ../assets/ds/ds_components.svg

[![][ds]](https://user.cyverse.org/services/3){target=_blank} 

**Figure**: Access to the Data Store can be accomplished over  HTTPS (`https://`), SFTP (`sftp://`) or iRODS Protocol 

## :octicons-database-24: iRODS

CyVerse uses iRODS (integrated Rule Oriented Data System) to manage its users data. 

[:octicons-database-24: iRODS Documentation](https://irods.org/documentation/)

[:simple-postgresql: PostgreSQL](https://github.com/cyverse-de/metadata-db) is used to implement the iCAT database.  

[:simple-github: :simple-ansible: Data Store Ansible Playbooks](https://github.com/cyverse/ds-playbooks)

## :material-web: SFTP

[Services details here]

## :material-web: WebDav

[Services details here]


## :octicons-cache-16: Caching

[Services Details here]

[:octicons-cache-16: Varnish-Cache](https://varnish-cache.org/) 