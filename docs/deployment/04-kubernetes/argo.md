---
type: Deployment Procedure
title: "Argo Workflows"
description: "Installing Argo Workflows and the workflow resources CyVerse analyses depend on."
tags: [deployment, kubernetes, argo, workflows]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
  - id: argo-docs
    resource: https://argo-workflows.readthedocs.io/
    title: Argo Workflows documentation
    author: team:argoproj
---

# Role in the deployment

Argo Workflows runs the containerized, non-interactive side of DE analyses inside
Kubernetes — the batch counterpart to VICE. Deployments that predate it dispatch
those jobs to HTCondor instead; the pilot leaves the `01_condor` inventory group
empty and uses Argo.

# Install

Two steps, in order:

```bash
ansible-playbook -i /path/to/inventory --tags argo kubernetes.yml
ansible-playbook -i /path/to/inventory argo_resources.yml
```

The first installs the controller and CRDs. The second creates the workflow
resources — service accounts, roles, and templates — that DE analyses submit
against. The resources playbook fails if the CRDs are not in place yet, so let
the first finish before starting the second.

# Verify

```bash
kubectl -n argo get pods
kubectl get crd | grep argoproj
kubectl -n argo get workflowtemplates
```

# Related

* [Cluster](./cluster.md)
* [Discovery Environment deployment](../06-applications/discovery-environment.md)
* [VICE](../06-applications/vice.md)
