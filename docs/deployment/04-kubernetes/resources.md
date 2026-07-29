---
type: Deployment Procedure
title: "Cluster resources"
description: "Generating and loading the configuration, secrets, and manifests every DE service reads."
tags: [deployment, kubernetes, configuration, secrets]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---
# What this repository holds

The cluster resources repository carries everything the DE services read at
runtime that is not part of a service image: the generated ConfigMaps and Secrets,
the kustomize bases and overlays, service accounts, roles, network policies, and
add-on manifests. Several other documents in this phase point back here for
manifests.

```bash
git clone <K8S_RESOURCES_REPO>
```

Throughout this document, `<ENV>` is the environment name in `config_values/`
(`prod` in a standard deployment) and `<NAMESPACE>` is the Kubernetes namespace
you are deploying into. They are frequently but not necessarily the same string.

## Tools

| Tool | Install |
|------|---------|
| [gomplate](https://docs.gomplate.ca/installing/) | Renders the configuration and secret templates |
| [skaffold](https://skaffold.dev/docs/install/) | Builds and deploys individual services |

```bash
sudo curl -o /usr/local/bin/gomplate -sSL \
  https://github.com/hairyhenderson/gomplate/releases/download/v3.10.0/gomplate_linux-amd64
sudo chmod 755 /usr/local/bin/gomplate
gomplate --help

curl -Lo skaffold \
  https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64
sudo install skaffold /usr/local/bin/
skaffold --help
```

Pin both to a known version for a production deployment rather than tracking
`latest`.

## Generate and load configuration

```bash
# render templates from config_values/<ENV>.yaml
./generate_configs.py -e <ENV>
./generate_secrets.py -e <ENV>

# load them into the cluster
./load_configs.py -e <ENV> -n <NAMESPACE>
./load_secrets.py -e <ENV> -n <NAMESPACE>
```

Rendered output contains every credential in the deployment. Keep it out of any
public repository, and re-render rather than hand-editing what is loaded.

## Prerequisites for deploying services

Registry pull secrets, created where the workloads that need them run:

| Secret | Namespace | Created by |
|--------|-----------|------------|
| `harbor-registry-credentials` | DE namespace | [Harbor](./harbor.md) |
| `vice-image-pull-secret` | `vice-apps` | [VICE](../06-applications/vice.md) |

Secrets and files the DE services expect, loaded by `load_secrets.py` from your
private secrets repository:

* `gpg-keys`
* `ui-nginx-tls`
* `pgpass-files`
* `signing-keys`
* `accepted-keys`
* `ssl-files`

Also required before the service set will come up:

* A search cluster — [OpenSearch](../05-core-services/opensearch.md), or
  [Elasticsearch](../05-core-services/elasticsearch.md) in older deployments.
* The service accounts applied:

    ```bash
    kubectl apply -f resources/serviceaccounts/app-exposer.yml -n <NAMESPACE>
    ```

## Deploy

Everything listed in `repos`:

```bash
./deploy.py -n <NAMESPACE> -BCa
```

A single service, for example `search`:

```bash
./deploy.py -Bn <NAMESPACE> -p search -C
```

In a full deployment, Ansible's `deploy-all-services` tag is the supported path;
`deploy.py` is for iterating on one service. See
[Discovery Environment](../06-applications/discovery-environment.md).

# Creating a new environment

Each environment is one file in `config_values/`. Copy an existing one and fill it
in for your site:

```bash
cp config_values/prod.yaml config_values/<ENV>.yaml
```

Every value has to be reviewed — the file is the single inventory of what the DE
services are configured with. The template below is that inventory; sections such
as `Keycloak`, `IRODS`, `Grouper`, `Harbor`, and the per-database blocks are
cross-referenced from the documents that produce their values.

```yaml
# config_values/<ENV>.yaml
---
Environment:

Agave:
  Key:
  Secret:
  RedirectURI:
  StorageSystem:
  CallbackBaseURI:
  ReadTimeout:
  Enabled:
  JobsEnabled:

AMQP:
  URI:

AnonFiles:
  BaseURI:

AppExposer:
  BaseURI:

BaseURLs:
  Analyses:
  Apps:
  AsyncTasks:
  DashboardAggregator:
  DataInfo:
  GrouperWebServices:
  IplantEmail:
  IplantGroups:
  JexAdapter:
  Metadata:
  Notifications:
  Permissions:
  Requests:
  Search:
  Terrain:
  UserInfo:

CAS:
  BaseURI:
  ServerName:
  UIDDomain:

DashboardAggregator:
  PublicGroup:
  LogLevel:

DataOne:
  BaseURI:

DE:
  Version:
  VersionName:
  AMQP:
    URI:
  Host:
  BaseURI:
  Legacy:
    BaseURI:
  Subscriptions:
    CheckoutURL:
  KeepAlive:
    Service:
    Target:
  ContextMenu:
    Enabled:
  BaseTrash:
    Path:
  ProdDeployment:
  DefaultOutputFolder:
  WSO2:
    JWTHeader:
  Coge:
    BaseURI:
  Tools:
    Admin:
      MaxCpuLimit:
      MaxMemoryLimit:
      MaxDiskLimit:

Docker:
  TrustedRegistries:
  Tag:

Elasticsearch:
  BaseURI:
  Username:
  Password:
  Index:

Email:
  AppDeletion:
    Src:
    Dest:
  AppPublicationRequest:
    Src:
    Dest:
  ToolRequest:
    Src:
    Dest:
  PermIDRequest:
    Src:
    Dest:
  Support:
    Src:
    Dest:

Grouper:
  Environment:
  MorphString:
  WebService:
    Password:
  Password:
  DB:
    User:
    Password:
    Host:
    Port:
    Name:
  FolderNamePrefix:
  Loader:
    URI:
    User:
    Password:
  SubjectSource:
    ID:
    Name:
    SearchBase:

ICAT:
  Host:
  Port:
  User:
  Password:

Infosquito:
  DayNum:
  PrefixLength:

InteractiveApps:
  BaseURI:
  ServiceSuffix:

Intercom:
  AppID:
  CompanyID:
  CompanyName:
  Intercom:

IRODS:
  AMQP:
    URI:
  Host:
  User:
  Zone:
  Password:
  AdminUsers:
  PermsFilter:
  ExternalHost:
  QuotaRootResources:

Jobs:
  DataTransferImage:

JobStatusListener:
  BaseURI:

Keycloak:
  ServerURI:
  Realm:
  ClientID:
  ClientSecret:
  VICE:
    ClientID:
    ClientSecret:

Kifshare:
  ExternalUri:

PGP:
  KeyPassword:

PermanentID:
  CuratorsGroup:
  DataCite:
    BaseURI:
    User:
    Password:
    DOIPrefix:

Redis:
  Host:
  Port:
  HA:
    Name:
  Password:
  DB:
    Number:

TimeZone:

Vault:
  Token:
  URL:
  IRODS:
    MountPath:
    ChildToken:
      UseLimit:

VICE:
  DB:
    User:
    Password:
    Host:
    Port:
    Name:
  FileTransfers:
    Image:
    Tag:
  JobStatus:
    BaseURI:
  K8sEnabled:
  BackendNamespace:
  ImagePullSecret:
  ImageCache:
  UseCSIDriver:
  DefaultImage:
  DefaultName:
  DefaultCasUrl:
  DefaultCasValidate:
  ConcurrentJobs:
  UseCaseCharsMin:
  DefaultBackend:
    LoadingPageTemplateString:

Sonora:
  BaseURI:

Terrain:
  CASClientID:
  CASClientSecret:
  JWT:
    SigningKey:
      Password:

Unleash:
  BaseUrl:
  APIPath:
  APIToken:
  MaintenanceFlag:

UserPortal:
  BaseURI:

DEDB:
  User:
  Password:
  Host:
  Port:
  Name:

NewNotificationsDB:
  User:
  Password:
  Host:
  Port:
  Name:

NotificationsDB:
  User:
  Password:
  Host:
  Port:
  Name:

PermissionsDB:
  User:
  Password:
  Host:
  Port:
  Name:

QMSDB:
  User:
  Password:
  Host:
  Port:
  Name:
  Reinitialize:

MetadataDB:
  User:
  Password:
  Host:
  Port:
  Name:

UnleashDB:
  User:
  Password:
  Host:
  Port:
  Name:

Admin:
  Groups:
  Attribute:

FileIdentifier:
  HtPathList:
  MultiInputPathList:

Analytics:
  Enabled:
  Id:

Harbor:
  URL:
  ProjectQARobotName:
  ProjectQARobotSecret:

QMS:
  Enabled:
  Base:
  Usage:

Jaeger:
  Endpoint:
```

# Related

* [Harbor](./harbor.md)
* [Discovery Environment](../06-applications/discovery-environment.md)
* [VICE](../06-applications/vice.md)
* [Keycloak](../05-core-services/keycloak.md)
