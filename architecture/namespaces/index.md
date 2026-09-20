---
type: Reference
title: "Kubernetes namespaces"
description: "Namespaces used by a CyVerse Kubernetes deployment and what runs in each one."
tags: [architecture, kubernetes, namespaces]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

# How namespaces are used

CyVerse groups workloads into namespaces along two lines: what has to be isolated
for security (user containers), and what has its own lifecycle (add-ons that are
installed and upgraded independently of the DE service set).

The DE service namespace is conventionally `prod`. A site running more than one
environment in a cluster names its namespaces after the environments; where a
document in this bundle says `<NAMESPACE>`, that is the choice it refers to.

![Namespaces](https://docs.cyverse.org/assets/namespaces.png)

# The DE service namespace

`prod` in a standard deployment. It holds the DE service set and the supporting
services the DE talks to directly:

| Workload | Document |
|----------|----------|
| DE services (Terrain, apps, analyses, metadata, notifications, search, UI) | [Discovery Environment](https://docs.cyverse.org/deployment/06-applications/discovery-environment/) |
| `de-nginx` front end | [Discovery Environment](https://docs.cyverse.org/deployment/06-applications/discovery-environment/) |
| Redis and Redis HAProxy | [Redis HA](https://docs.cyverse.org/deployment/05-core-services/redis-ha/) |
| Search cluster | [OpenSearch](https://docs.cyverse.org/deployment/05-core-services/opensearch/) |
| Grouper loader and web services | [Grouper](https://docs.cyverse.org/deployment/05-core-services/grouper/) |
| Unleash | [Unleash](https://docs.cyverse.org/deployment/05-core-services/unleash/) |
| NATS | [NATS](https://docs.cyverse.org/deployment/05-core-services/nats/) |
| User Portal (or its own `user-portal` namespace) | [User Portal](https://docs.cyverse.org/deployment/06-applications/user-portal/) |

# Dedicated namespaces

| Namespace | Contents | Why it is separate |
|-----------|----------|--------------------|
| `vice-apps` | Interactive analyses, `app-exposer`, the VICE operator | User-supplied containers need their own network policy and service accounts — see [VICE](https://docs.cyverse.org/deployment/06-applications/vice/) |
| `keycloak` | Keycloak | Authentication is upgraded on its own schedule — see [Keycloak](https://docs.cyverse.org/deployment/05-core-services/keycloak/) |
| `openldap` | OpenLDAP | System of record for accounts — see [OpenLDAP](https://docs.cyverse.org/deployment/05-core-services/openldap/) |
| `irods-csi-driver` | The iRODS CSI driver | Node-level storage plugin with its own upgrade procedure — see [iRODS CSI driver](https://docs.cyverse.org/deployment/05-core-services/irods-csi-driver/) |
| `longhorn-system` | Longhorn | Cluster storage — see [storage](https://docs.cyverse.org/deployment/04-kubernetes/storage/) |
| `openebs` | OpenEBS (legacy) | Older cluster storage — see [storage](https://docs.cyverse.org/deployment/04-kubernetes/storage/) |
| `ingress-nginx` | ingress-nginx | VICE ingresses; being retired — see [ingress](https://docs.cyverse.org/deployment/04-kubernetes/ingress/) |
| `cert-manager` | cert-manager and cluster issuers | TLS issuance — see [cert-manager](https://docs.cyverse.org/deployment/04-kubernetes/cert-manager/) |
| `argo` | Argo Workflows | Batch analyses — see [Argo](https://docs.cyverse.org/deployment/04-kubernetes/argo/) |
| `harbor` | Harbor registry | Registry lifecycle — see [Harbor](https://docs.cyverse.org/deployment/04-kubernetes/harbor/) |
| `mail` | exim4 smarthost | Optional; see [mail](https://docs.cyverse.org/deployment/05-core-services/mail/) |
| `jaeger` | Jaeger collector, query, rollover cron | Optional tracing — see [Jaeger](https://docs.cyverse.org/deployment/05-core-services/jaeger/) |

Which of these exist depends on what you deployed: Longhorn or OpenEBS, OpenSearch
or Elasticsearch, mail and tracing only if installed.

# Practical notes

* **Namespaced manifests.** Several manifests in
  [cluster resources](https://docs.cyverse.org/deployment/04-kubernetes/resources/) carry a namespace
  in a kustomization or in an argument. Deploying into a namespace other than the
  default means changing them; each document flags where.
* **Cross-namespace addresses.** In-cluster names are
  `<service>.<namespace>` — `ldap://openldap.openldap` for the directory,
  `http://vice-operator.vice-apps:10000` for the VICE operator.
* **`kubectl` scope.** Most troubleshooting starts with
  `kubectl get pods -A`; per-namespace commands in this bundle use `<NAMESPACE>`
  wherever the value is a site choice.

# Related

* [System overview](https://docs.cyverse.org/architecture/system-overview/)
* [Deployment](https://docs.cyverse.org/deployment/)
* [Network requirements](https://docs.cyverse.org/architecture/network-requirements/)
