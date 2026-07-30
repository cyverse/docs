---
type: Deployment Procedure
title: "Discovery Environment"
description: "Deploying the DE service set and the nginx front end that proxies it."
tags: [deployment, applications, discovery-environment]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: pilot-record
    resource: ../../references/pilot-deployment-record.md
    title: Pilot CyVerse deployment record
    author: process:cyverse-devops
    last_modified: 2026-07-29
---

# Prerequisites

Everything in phases 1 through 5. Specifically:

* [Databases](../02-databases/index.md) created and migrated.
* [iRODS integration](../03-data-store/de-integration.md) complete — specific
  queries installed and the `de-irods` account created.
* [Keycloak](../05-core-services/keycloak.md) configured, with every client
  secret written into `group_vars/all.yml`.
* Service signing keys generated (`./scripts/generate-secrets.sh`) and the
  printed YAML snippet added to the group variables.
* [Cluster resources](../04-kubernetes/resources.md) loaded — in particular the
  `harbor-registry-credentials` and `de-nginx-tls` secrets in the DE namespace.
* If HAProxy terminates TLS against a private CA, that CA is in the HAProxy
  host's trust bundle (`/etc/ssl/certs/ca-bundle.crt` on RPM-based hosts).

# Deploy the service set

The whole DE — Terrain, apps, analyses, metadata, notifications, search, the
Sonora UI, and the VICE backend — is deployed by one tag:

```bash
ansible-playbook -i /path/to/inventory --tags=deploy-all-services kubernetes.yml
```

Watch for anything that does not settle:

```bash
kubectl get pods -A | grep -Ev 'Running|Completed'
```

`ImagePullBackOff` on an image that exists points at the registry pull secret,
not the registry. `CrashLoopBackOff` on a service that starts and immediately
exits is usually a missing configuration key or an unreachable database.

# The nginx front end

`de-nginx` proxies the DE's services behind a single hostname. Its manifests are
in the [cluster resources](../04-kubernetes/resources.md) repository as a
kustomize base with per-environment overlays.

Two values in the base are site-specific and have to match your deployment:

**`resources/kustomize/de-nginx/base/nginx.conf`** — the server name regex:

```diff
- server_name ~^[^.]+[.]example[.]org$;
+ server_name ~^[^.]+[.]<BASE_DOMAIN_ESCAPED>$;
```

The dots are escaped as `[.]` because the value is a regex. `<BASE_DOMAIN>`
written literally would match more hostnames than you intend.

**`resources/kustomize/de-nginx/base/kustomization.yaml`** — the namespace:

```diff
- namespace: prod
+ namespace: <NAMESPACE>
```

Then apply the overlay and the service definition for your site:

```bash
kubectl apply -k resources/kustomize/de-nginx/overlays/<OVERLAY>/ -n <NAMESPACE>
kubectl apply -f resources/services/<SITE>.yml -n <NAMESPACE>
```

# Verify

1. `https://de.<BASE_DOMAIN>` loads and redirects to Keycloak for sign-in.
2. Sign-in returns you to the DE with your account name in the UI.
3. The data browser lists your Data Store home directory.

If sign-in loops back to Keycloak, the client's redirect URIs or web origins are
wrong; see [Keycloak](../05-core-services/keycloak.md#clients).

# Next

* [VICE](./vice.md) — interactive analyses
* [User Portal](./user-portal.md) — account management
* [Bootstrap](../07-post-install/bootstrap.md) — first administrator and apps
