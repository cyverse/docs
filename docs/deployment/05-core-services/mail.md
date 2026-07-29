---
type: Deployment Procedure
title: "Mail"
description: "Deploying outbound mail: the exim4 smarthost chart and the in-cluster exim-sender deployment."
tags: [deployment, core-services, mail, exim, smtp]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: exim4-helm
    resource: https://github.com/mb-wali/exim4-helm
    title: exim4 Helm chart
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
---

# What needs mail

The DE sends mail for app publication requests, tool requests, permanent ID
requests, app deletion notices, and support messages. The destinations are
configured in the `Email` section of the deployment group variables; see
[cluster resources](../04-kubernetes/resources.md). The User Portal also sends
account verification mail.

Two deployments exist, and they are alternatives rather than layers. Pick the one
that matches how your site relays mail.

| Option | Use when |
|--------|----------|
| exim4 smarthost (Helm) | You relay through an institutional or provider SMTP smarthost |
| exim-sender (manifest) | You want a minimal in-cluster sender managed with the other DE manifests |

Both present an SMTP endpoint inside the cluster that DE services point at
through `SMTP_HOST`.

# Option 1: exim4 smarthost

A Helm chart providing exim4 as a mail transfer agent in smarthost
mode.[^exim4-helm]

```bash
helm repo add exim4 https://mb-wali.github.io/exim4-helm
helm repo update

helm install exim4 exim4/exim4 \
  --namespace mail --create-namespace --wait \
  --set secrets.EXIM_SMARTHOST='<SMARTHOST_HOST>' \
  --set secrets.EXIM_PASSWORD='<GENERATED_SECRET>' \
  --set secrets.EXIM_ALLOWED_SENDERS='<ALLOWED_SENDER_PATTERN>'
```

!!! warning "Values on the command line are not private"

    `--set` puts the smarthost password into your shell history and into the Helm
    release. Prefer a values file kept in the private inventory, or a
    pre-created secret that the chart references.

    `EXIM_ALLOWED_SENDERS='*'` appears in older notes. It permits relaying from
    any sender; scope it to your own domains instead.

In-cluster endpoint:

```
SMTP_HOST=exim4.mail.svc.cluster.local
```

## Verify

```bash
kubectl -n mail get pods
kubectl -n mail exec -it deploy/exim4 -- bash

# from inside the pod
echo "This is a test" | mail -s "Test subject" \
    you@<BASE_DOMAIN> -aFrom:noreply@<BASE_DOMAIN>
```

Then check the exim logs in the pod for the delivery result. A message accepted
locally but never delivered is usually the smarthost rejecting the envelope
sender.

# Option 2: exim-sender

Also known as `local-exim`, deployed from the manifests in
[cluster resources](../04-kubernetes/resources.md) alongside the other DE
services:

```bash
kubectl apply -f resources/deployments/exim-sender.yml -n <NAMESPACE>
```

Use the namespace the DE services run in (`prod` in a standard deployment).

# Related

* [Cluster resources](../04-kubernetes/resources.md)
* [User Portal](../06-applications/user-portal.md)

[^exim4-helm]: exim4 Helm chart
