# :material-terrain: Terrain

Terrain provides the primary point of communication between the Discovery Environment (DE) UI and backend services. 

It's charged with two primary tasks: 

(1) to handle authentication and authorization for endpoints that require it, 

(2) to orchestrate calls to other lower-level services.

The primary and most current source for Terrain API endpoint documentation are the Swagger docs hosted by the service itself:
[https://de.cyverse.org/terrain/docs](https://de.cyverse.org/terrain/docs){target=_blank}

Not all endpoints have been documented in those Swagger docs yet,
and those endpoints will be listed under the
[default](https://de.cyverse.org/terrain/docs/index.html#!/default)
Swagger category.
The documentation for those endpoints may still be found below.

* [Endpoints](./api/endpoints/endpoints.md)

* [Endpoints Index](./api/endpoint-index.md)

* [Errors](./api/errors.md)

## Authentication

Authentication to the DE services is currently handled by
[Keycloak](http://www.keycloak.org/){target=_blank},
allowing clients to obtain OAuth or OIDC tokens for accessing secured API endpoints.

See the [/terrain/token/keycloak](https://de.cyverse.org/terrain/docs/index.html#!/token/get_terrain_token_keycloak)
endpoint docs for details on obtaining tokens.

## [Tapis v2 App migration to v3 for the Discovery Environment](./api/tapis-v2-v3-migration.md)
