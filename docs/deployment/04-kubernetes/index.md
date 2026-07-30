# Phase 4: Kubernetes

The cluster and the add-ons every service above it assumes. Install in this order —
cert-manager before ingress so routes can reference their certificates, storage
before anything stateful.

* [Cluster](cluster.md) - control plane and workers with k0sctl
* [Cluster resources](resources.md) - generated configuration, secrets, and manifests
* [cert-manager](cert-manager.md) - TLS issuance, including the VICE wildcard
* [Ingress](ingress.md) - HAProxy, Traefik, and the legacy ingress-nginx path
* [Storage](storage.md) - persistent volumes with Longhorn or OpenEBS
* [Harbor](harbor.md) - the container registry
* [Argo Workflows](argo.md) - batch analysis execution

# Related

* [Kubernetes namespaces](../../architecture/namespaces.md)
* [Network requirements](../../architecture/network-requirements.md)

# Next

* [Phase 5: core services](../05-core-services/)
