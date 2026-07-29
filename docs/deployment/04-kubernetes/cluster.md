---
type: Deployment Procedure
title: "Cluster"
description: "Standing up the Kubernetes control plane and workers that run CyVerse services, with k0sctl."
tags: [deployment, kubernetes, k0s, cluster]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: k0sctl
    resource: https://docs.k0sproject.io/stable/k0sctl-install/
    title: k0sctl installation and cluster configuration
    author: team:k0sproject
---

# Prerequisites

* [Prerequisites](../planning/prerequisites.md) and
  [Ansible](../planning/ansible.md) complete.
* [Foundation services](../01-foundation/index.md) installed, and
  [iRODS](../03-data-store/irods-provider.md) running.
* Passwordless `ssh` from your workstation to every node, and `sudo` on each.
* The Kubernetes ports in
  [network requirements](../../architecture/network-requirements.md) open
  between nodes.

# Inventory

The cluster's shape comes from the Ansible inventory. For a two-node pilot:

| Inventory group | Hosts |
|-----------------|-------|
| `k8s_api_proxy` | `core-1` |
| `k8s_controllers` | `core-1` |
| `k8s_de_workers` | `core-1` |
| `k8s_vice_workers` | `analysis-1` |

A node can appear in several groups; `core-1` is control plane and DE worker at
once. VICE workers stay separate so interactive analyses cannot starve the
service set.

# Prepare the nodes

```bash
ansible-playbook -i /path/to/inventory --tags prep-nodes kubernetes.yml
```

This installs the packages, kernel modules, and sysctl settings the container
runtime needs, and applies host firewall rules.

# Create the cluster with k0sctl

## k0sctl.yaml

`k0sctl.yaml` lists the hosts and their roles. Keep it in your private
inventory repository — it names hosts and paths and identifies your SSH user.

```yaml
apiVersion: k0sctl.k0sproject.io/v1beta1
kind: Cluster
metadata:
  name: <SITE>-cluster
spec:
  hosts:
    - role: controller+worker
      noTaints: true
      ssh:
        address: core-1.<BASE_DOMAIN>
        user: <SSH_USER>
        keyPath: /path/to/private-key
    - role: worker
      ssh:
        address: analysis-1.<BASE_DOMAIN>
        user: <SSH_USER>
        keyPath: /path/to/private-key
  k0s:
    version: <K0S_VERSION>
    config:
      spec:
        network:
          provider: calico
        telemetry:
          enabled: false
```

Treat this as a skeleton: pin `<K0S_VERSION>` to the release you tested, and add
the node labels, taints, and API server SANs your site needs. The authoritative
reference for the fields is the k0sctl documentation.[^k0sctl]

!!! note "Where the example belongs"

    Earlier notes pointed at an example `k0sctl.yaml` shared in a chat channel.
    Keep the example in the deployment repository beside the playbooks instead —
    a cluster definition that only exists in chat history is a cluster nobody
    can rebuild.

## Apply

```bash
export K0S_SSH_USER=<SSH_USER>
export K0S_SSH_KEY_PATH=/path/to/private-key
export KUBECONFIG="$HOME/.kube/config"
mkdir -p "$(dirname "$KUBECONFIG")"
k0sctl apply --config /path/to/k0sctl.yaml
```

!!! warning "`dirname`, not `basename`"

    The directory to create is the *parent* of the kubeconfig path. Earlier
    notes used `basename`, which creates a directory named `config` and leaves
    `k0sctl` writing to a path that does not exist.

## Untaint a combined control-plane node

When the control-plane node is also a DE worker, k0s may have tainted it. On the
control node:

```bash
k0s kubectl taint node core-1 node-role.kubernetes.io/control-plane:NoSchedule-
```

If the taint was never applied, the command says so. That message is the
expected outcome, not a problem to investigate.

## Verify

```bash
kubectl get nodes -o wide
kubectl get pods -A
```

Every node `Ready`, and no pod outside `Running` or `Completed`, before moving
on.

# Legacy: Ansible-provisioned clusters

Deployments predating k0sctl built the cluster with the playbooks in
[cyverse-de/deployments](https://github.com/cyverse-de/deployments/tree/main/ansible/kubernetes)
against CentOS 7 hosts. The inventory looked like this:

```ini
[k8s:children]
k8s-control-plane
k8s-worker

[kube-apiserver-haproxy]
k8s-reverse-proxy.<BASE_DOMAIN>

[k8s-control-plane]
k8s-c1.<BASE_DOMAIN>

[k8s-storage:children]
k8s-worker

[k8s-worker]
k8s-w1.<BASE_DOMAIN>
k8s-w2.<BASE_DOMAIN>
vice-w1.<BASE_DOMAIN>

[vice-workers]
vice-w1.<BASE_DOMAIN>
```

with `firewalld-config.yml` and `provision-nodes.yml`, and VICE workers labelled
and tainted by hand:

```bash
kubectl label nodes vice-w1.<BASE_DOMAIN> vice=true
kubectl taint nodes vice-w1.<BASE_DOMAIN> vice=only:NoSchedule
```

CentOS 7 is end of life, so this path is documented for reading existing
clusters rather than for building new ones. New deployments use k0sctl, and node
labelling is handled by the `prep-nodes` tag and node feature discovery.

# Next

* [Cluster resources](./resources.md)
* [cert-manager](./cert-manager.md)
* [Storage](./storage.md)
* [Ingress](./ingress.md)

[^k0sctl]: k0sctl installation and cluster configuration
