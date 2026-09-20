# Phase 4: Kubernetes

The cluster and the add-ons every service above it assumes. Install in this order —
cert-manager before ingress so routes can reference their certificates, storage
before anything stateful.

* [Cluster](https://docs.cyverse.org/deployment/04-kubernetes/cluster/) - control plane and workers with k0sctl
* [Cluster resources](https://docs.cyverse.org/deployment/04-kubernetes/resources/) - generated configuration, secrets, and manifests
* [cert-manager](https://docs.cyverse.org/deployment/04-kubernetes/cert-manager/) - TLS issuance, including the VICE wildcard
* [Ingress](https://docs.cyverse.org/deployment/04-kubernetes/ingress/) - HAProxy, Traefik, and the legacy ingress-nginx path
* [Storage](https://docs.cyverse.org/deployment/04-kubernetes/storage/) - persistent volumes with Longhorn or OpenEBS
* [Harbor](https://docs.cyverse.org/deployment/04-kubernetes/harbor/) - the container registry
* [Argo Workflows](https://docs.cyverse.org/deployment/04-kubernetes/argo/) - batch analysis execution

# Related

* [Kubernetes namespaces](https://docs.cyverse.org/architecture/namespaces/)
* [Network requirements](https://docs.cyverse.org/architecture/network-requirements/)

# Next

* [Phase 5: core services](https://docs.cyverse.org/deployment/05-core-services)
