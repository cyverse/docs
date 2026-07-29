---
type: Playbook
title: "Verification"
description: "The end-to-end checks that tell you a fresh CyVerse deployment actually works."
tags: [deployment, post-install, verification, testing]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---

# How to use this

Work down the list. Each check depends on the ones above it, so a failure is
usually caused by the last thing that passed rather than by the check that broke.

# Cluster

```bash
kubectl get nodes -o wide
kubectl get pods -A | grep -Ev 'Running|Completed'
kubectl get pvc -A
kubectl get certificates -A
```

* [ ] Every node `Ready`.
* [ ] No pod outside `Running` or `Completed`.
* [ ] No PVC `Pending` — a pending claim means the storage class cannot satisfy
      it; see [storage](../04-kubernetes/storage.md).
* [ ] Every certificate `True` / `Ready`; see
      [cert-manager](../04-kubernetes/cert-manager.md).

# Foundation

* [ ] `psql` connects from an admin host and from a pod.
* [ ] The RabbitMQ management UI is reachable from an admin host and **not** from
      outside.
* [ ] `iinit` and `ils` succeed as an ordinary user.
* [ ] `ils /<IRODS_ZONE>/home/public` succeeds as `anonymous`, with no password.

# Authentication

* [ ] `https://keycloak.<BASE_DOMAIN>` serves a valid certificate.
* [ ] The realm's LDAP federation **Test connection** and **Test authentication**
      both pass.
* [ ] A synced user appears under **Users** in the realm.
* [ ] A token issued for the `de-<SITE>` client carries both `name` and
      `entitlement` claims — this is what the client scope mappers exist for; see
      [Keycloak](../05-core-services/keycloak.md#client-scope-mappers).

# Discovery Environment

* [ ] `https://de.<BASE_DOMAIN>` loads and redirects to Keycloak.
* [ ] Sign-in returns to the DE with your account name shown.
* [ ] The data browser lists your Data Store home directory.
* [ ] Upload a small file through the DE, then confirm it with `ils` — this
      exercises the DE, iRODS, and the transfer path together.
* [ ] Search finds the uploaded file after a few moments. If not, follow the
      event chain in
      [iRODS integration](../03-data-store/de-integration.md#event-flow).

# Batch analysis

* [ ] Run **DE Word Count** on a small text file.
* [ ] The analysis completes and writes an output folder to the Data Store.
* [ ] A completion notification appears in the DE.

A batch analysis touches Argo (or HTCondor), the transfer credentials, the message
bus, and notifications, which is why it is the single most informative check here.

# Interactive analysis

* [ ] Launch **Cloud Shell**.
* [ ] The analysis URL under `*.vice.<BASE_DOMAIN>` resolves and serves a valid
      certificate.
* [ ] The shell opens and the user's Data Store home is mounted.
* [ ] Terminating the analysis removes the deployment, service, and ingress:

    ```bash
    kubectl -n vice-apps get deploy,svc,ingress
    ```

If the URL resolves but the certificate is wrong, the wildcard certificate is the
suspect; if the shell opens without the home directory, look at the
[iRODS CSI driver](../05-core-services/irods-csi-driver.md).

# User Portal

* [ ] `https://user.<BASE_DOMAIN>` loads and signs in.
* [ ] A test account request appears in the admin panel.
* [ ] Approving it produces a working account: it can sign in to the DE and has
      an iRODS home collection.
* [ ] Account verification mail arrives; see
      [mail](../05-core-services/mail.md).

# Operational readiness

Not required for the deployment to work, and required before anyone relies on it:

* [ ] Database backups scheduled and a restore tested.
* [ ] iRODS vault backup or replication in place.
* [ ] Certificate renewal verified against the staging issuer.
* [ ] Log rotation confirmed for iRODS
      ([provider](../03-data-store/irods-provider.md#configure-logging-first)),
      and index rollover confirmed if [Jaeger](../05-core-services/jaeger.md) is
      deployed.
* [ ] Deployment secrets stored only in the private inventory repository.

# Related

* [Troubleshooting](./troubleshooting.md)
* [FAQ](../../operations/faq.md)
