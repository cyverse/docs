---
type: Architecture Overview
title: "Authentication"
description: "How Keycloak, CILogon, and OAuth 2.0 combine to authenticate CyVerse users."
tags: [platform, authentication, keycloak]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---
# How CyVerse authenticates users

CyVerse authenticates through [Keycloak](https://www.keycloak.org/){target=_blank},
which brokers to [CILogon](https://cilogon.org/){target=_blank} for federated
institutional identity and to OAuth 2.0 providers such as Google, GitHub, and
ORCID.

``` mermaid
sequenceDiagram
  autonumber
  User->>Browser: Click on external Auth
  Browser-->>Keycloak: Authentication request (TOKEN)
  loop
      Keycloak-->>Browser: Browser opened with ../auth?=client_id=de-prod=TOKEN
  end
  Keycloak-->>CILogon: Auth Request
  User->>CILogon: Enter Credentials
  Keycloak-->>OAUTH: Auth Response
  CILogon-->>Keycloak: Auth Response
  Browser-->>OAUTH: Ask for Token
  OAUTH-->>Browser: Retrieve Token
```

[comment]: https://docs.cyverse.org/platform/<> (![keycloak](https://docs.cyverse.org/assets/de/keycloak.svg))

Users start at the left: the browser is redirected to Keycloak, which brokers the request to CILogon or an OAuth provider, and the token comes back through the browser.

The public US deployment runs Keycloak at
[kc.cyverse.org](https://kc.cyverse.org){target=_blank}.

## Deployment

Keycloak runs in the Kubernetes cluster, in its own namespace. Deploying and
configuring it — realm, LDAP federation, mappers, realm roles, and the OAuth
clients each CyVerse application needs — is covered in
[Keycloak deployment](https://docs.cyverse.org/deployment/05-core-services/keycloak/). Its database is
[the Keycloak database](https://docs.cyverse.org/deployment/02-databases/keycloak/), and the account
directory it federates is [OpenLDAP](https://docs.cyverse.org/deployment/05-core-services/openldap/).

## Using it from an API client

Terrain accepts OAuth and OIDC tokens issued by Keycloak; see
[Terrain](https://docs.cyverse.org/api/terrain/) for how clients obtain one.


The provisioning playbooks live in
[ansible-kubernetes-keycloak](https://github.com/cyverse/ansible-kubernetes-keycloak){target=_blank}.

## Related

* [Keycloak deployment](https://docs.cyverse.org/deployment/05-core-services/keycloak/)
* [OpenLDAP](https://docs.cyverse.org/deployment/05-core-services/openldap/)
* [Terrain](https://docs.cyverse.org/api/terrain/)
