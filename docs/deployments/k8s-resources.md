# k8s-resources

This repository includes all the manifests and resources for kubernetes deployment.

!!! success "Prerequisites"


## Clone

```bash
git clone git@gitlab.cyverse.org:tugraz/k8s-resources.git
```

## Generate config/secrets for **prod** env

Make sure [gomplate](https://docs.gomplate.ca/installing/#manual-install) is installed in your OS.

**Here is an example on ubuntu:**

```bash
sudo curl -o /usr/local/bin/gomplate -sSL https://github.com/hairyhenderson/gomplate/releases/download/v3.10.0/gomplate_linux-amd64

sudo chmod 755 /usr/local/bin/gomplate

gomplate --help
```

**Generate configs/secrets**
```bash
./generate_configs.py -e prod
./generate_secrets.py -e prod
```

## Load config/secrets in your cluster for **prod** env

```bash
./load_configs.py -e prod -n prod
./load_secrets.py -e prod -n prod
```

## Deploy services

### preq

Before we start to deploy the services we need to create these secrets, and install some dependencies.

* `vice-image-pull-secret` TODO: kubectl apply.
* `harbor-registry-credentials` TODO: kubectl apply.

**These secrets below can be deployed using:** `git clone -b discover https://gitlab.cyverse.org/tugraz/docker-tugraz-data` see also [cyverse.at](https://github.com/mb-wali/cyverse.at/tree/main/k8s/discoveryEnvironment) repo.

* `gpg-keys`
* `ui-nginx-tls`
* `gpg-keys`
* `pgpass-files`
* `signing-keys`
* `accepted-keys`
* `ssl-files`
* Make sure [elasticsearch](elasticsearch.md) is deployed.
* Make sure [skaffold](https://docs.gomplate.ca/installing/#manual-install) is installed in your OS.
    ```bash
    # For Linux x86_64 (amd64)
    curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64

    sudo install skaffold /usr/local/bin/

    # check
    skaffold --help
    ```
* Make sure ServiceAccounts are created:
  ```bash
  # For prod env
  kubectl apply -f /k8s-resources/resources/serviceaccounts/app-exposer.yml -n prod

  ## For discover env
  # kubectl apply -f /k8s-resources/resources/serviceaccounts/app-exposer.yml -n discover
  ```

#### TODO above

### Deploy

This command will deploy all the services listed on `k8s-resources/repos`

```bash
# deploy services for prod env
./deploy.py -n prod -BCa

## deploy services for discover env
# ./deploy.py -n discover -BCa
```

#### Deploy single service

If you want to deploy a single service, e.g. `search`
```bash
## Run for the prod env
./deploy.py -Bn prod -p search -C

## Run for the discover env
# ./deploy.py -Bn discover -p search -C
```

# create a new env
Currently we have `prod` environment which is our productive instance.

Create your environment: `cp config_values/prod.yaml config_values/discover.yaml`
This will create a new config file for your environment. You will have to update all the values as you see fit for your environment.

**fill in all these values:**
```yaml
# discover.yaml
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

## Generate config/secrets for **discover** env
```bash
./generate_configs.py -e discover
./generate_secrets.py -e discover
```

## Load config/secrets in your cluster for **discover** env
```bash
./load_configs.py -e discover -n discover
./load_secrets.py -e discover -n discover
```
