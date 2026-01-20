# Frequently Asked Questions (FAQ)

Common questions about deploying, administering, and developing with CyVerse infrastructure.

---

## Deployment & Installation

### How do I deploy CyVerse on my infrastructure?

Start with the [Getting Started Guide](../services/getting_started.md) to review prerequisites, then follow the [Deployment Guide](../deployments/deployment_overview.md) for step-by-step deployment instructions. The deployment follows this sequence:

1. Deploy Kubernetes cluster
2. Set up storage (OpenEBS) and networking (Ingress NGINX)
3. Deploy core services (KeyCloak, RabbitMQ, Redis, ElasticSearch)
4. Provision databases
5. Deploy application services (Discovery Environment, User Portal, VICE)

See the [deployment roadmap](../services/getting_started.md#deployment-roadmap) for detailed phase breakdown.

### What are the minimum hardware requirements?

**Minimum recommended configuration:**

- 8-node Kubernetes cluster
- 32 CPU cores per node (256 cores total)
- 128GB RAM per node (1TB total)
- 10TB+ persistent storage (NFS, iRODS, or cloud block storage)
- 10 Gbps network connectivity
- Public IP addresses for ingress services

**For smaller deployments** (testing/development):
- 3-node cluster
- 16 cores per node
- 64GB RAM per node
- 1TB storage

See [Prerequisites](../services/getting_started.md#prerequisites) for complete requirements.

### Can I deploy CyVerse on commercial cloud providers (AWS, GCP, Azure)?

Yes! CyVerse can be deployed on:

- :simple-amazonaws: **AWS** - Use EKS for Kubernetes, EBS for storage
- :simple-googlecloud: **Google Cloud** - Use GKE for Kubernetes, Persistent Disks for storage
- :simple-azuredevops: **Azure** - Use AKS for Kubernetes, Azure Disks for storage
- :simple-openstack: **OpenStack** - CyVerse's primary cloud platform

You'll need to adapt storage provisioning and load balancer configurations for your cloud provider. See the [Kubernetes deployment guide](../deployments/kubernetes-deploy.md).

### What Kubernetes distribution should I use?

CyVerse is tested with:

- **Upstream Kubernetes** (1.23+) - Recommended for most deployments
- **OpenShift** - Enterprise Kubernetes with additional security features
- **Rancher** - Kubernetes management platform
- **Cloud-managed Kubernetes** - EKS, GKE, AKS

Use the latest stable Kubernetes version supported by your distribution.

### How long does a full CyVerse deployment take?

**Typical timeline** (experienced DevOps team):

- **Weeks 1-2**: Infrastructure setup (Kubernetes, storage, networking)
- **Week 3**: Core services deployment (KeyCloak, RabbitMQ, Redis, ElasticSearch)
- **Week 4**: Database provisioning and configuration
- **Weeks 5-6**: Application deployment (DE, User Portal, VICE)
- **Week 7**: Testing, tuning, and documentation

**Total: 7-8 weeks** for initial production deployment.

Development/testing environments can be deployed faster (2-3 weeks).

### Do I need to deploy all CyVerse services?

No! CyVerse is modular. You can deploy:

- **Core only**: Discovery Environment for computational workflows
- **Data Store only**: iRODS-based data management
- **Custom subset**: Select services for your use case

All services require Kubernetes, PostgreSQL databases, and KeyCloak authentication.

### How do I update/upgrade CyVerse services?

Updates are managed through:

1. **Helm chart updates** - Pull latest charts from repository
2. **Container image updates** - Reference new image tags in deployments
3. **Database migrations** - Run migration scripts before deploying new versions
4. **Rolling updates** - Kubernetes handles zero-downtime deployments

Always test updates in a staging environment first. See [deployment documentation](../deployments/deployment_overview.md).

---

## Administration

### How do I grant VICE access to a user?

VICE (Visual Interactive Computing Environment) access is granted through the Discovery Environment admin interface:

1. Log into the Discovery Environment as an administrator
2. Navigate to Admin → User Management
3. Search for the user
4. Grant "VICE Access" permission
5. User must meet requirements: valid account, accepted terms of service, sufficient quota

See [DE Administration Guide](de.md#vice-access) for detailed procedures.

### How do I process a DOI/Permanent ID request?

DOI requests for data publishing follow this workflow:

1. User submits request through Discovery Environment (Data → Permanent ID Request)
2. Admin receives notification
3. Review submission in admin interface for completeness
4. Validate metadata and data quality
5. Approve request - system creates DOI via DataCite
6. Notify user of published DOI

See [Permanent ID Requests documentation](../services/api/endpoints/permanent-id-requests.md) for complete SOP.

### How do I add a new application to the Discovery Environment?

**Publishing a containerized app to DE:**

1. **Containerize your tool** - Create Docker image with tool/software
2. **Push to registry** - Upload to Docker Hub or Harbor
3. **Create integration** - Use DE "Apps" interface to define:
   - Input files
   - Parameters
   - Output files
   - Resource requirements
4. **Test privately** - Validate app functionality
5. **Publish publicly** - Make available to all users

See [DE Administration Guide](de.md#app-publication) for step-by-step instructions.

### How do I manage user storage quotas?

Storage quotas are managed through:

- **iRODS resource management** - Set per-user/per-group quotas in iRODS
- **User Portal** - Display quota usage to users
- **Admin tools** - Monitor and adjust quotas as needed

Contact users approaching quota limits and provide options (clean up data, request increase, archive to cold storage).

### How do I back up CyVerse databases?

**Database backup strategy:**

```bash
# PostgreSQL backup example
pg_dump -h db-host -U username -d de_database > de_backup.sql

# Automated daily backups
0 2 * * * /usr/local/bin/backup-cyverse-dbs.sh
```

Backup all service databases:
- DE database
- Metadata database
- Notifications database
- KeyCloak database
- QMS database
- Grouper database
- Portal database
- Unleash database

See [Database documentation](../database/main.md) for detailed backup procedures.

### How do I troubleshoot service failures?

**Systematic troubleshooting approach:**

1. **Check pod status**: `kubectl get pods -n <namespace>`
2. **View logs**: `kubectl logs -n <namespace> <pod-name>`
3. **Check events**: `kubectl describe pod -n <namespace> <pod-name>`
4. **Verify dependencies**: Ensure databases, RabbitMQ, Redis are accessible
5. **Check resource limits**: CPU/memory constraints can cause crashes
6. **Review recent changes**: Configuration updates, deployments, network changes

Common issues:
- Database connection failures (check credentials, network)
- Out of memory (increase resource limits)
- Image pull errors (check registry access)
- Configuration errors (validate YAML syntax)

### How do I add a new administrator?

Administrator permissions are managed through KeyCloak:

1. Log into KeyCloak admin console
2. Navigate to Users
3. Find or create the user
4. Assign admin role:
   - `de-admin` for Discovery Environment
   - `data-store-admin` for Data Store
   - `super-admin` for platform-wide access
5. User logs out and back in to activate permissions

See [KeyCloak documentation](../services/keycloak.md) for role management.

---

## API & Development

### How do I authenticate with the Terrain API?

Terrain uses **OAUTH 2.0** authentication via KeyCloak:

**Method 1: Get token via browser (authorization code flow)**
1. Direct user to KeyCloak authorization URL
2. User authorizes your application
3. Exchange authorization code for access token

**Method 2: Get token via command line (password flow)**
```bash
TOKEN=$(curl -s -X POST \
  https://kc.cyverse.org/auth/realms/CyVerse/protocol/openid-connect/token \
  -d "grant_type=password" \
  -d "client_id=de-client" \
  -d "username=YOUR_USERNAME" \
  -d "password=YOUR_PASSWORD" \
  | jq -r '.access_token')
```

**Use token in API calls:**
```bash
curl -H "Authorization: Bearer $TOKEN" \
  https://de.cyverse.org/terrain/filesystem/list
```

See [Developer Guide](../development/index.md#authentication) for complete examples.

### Where can I find the complete API documentation?

- **[Live Swagger UI](https://de.cyverse.org/terrain/docs/){target=_blank}** - Interactive API testing
- **[Endpoint Index](../services/api/endpoint-index.md)** - Complete endpoint reference
- **[API Overview](../services/api_overview.md)** - Terrain API introduction
- **[Error Codes](../services/api/errors.md)** - Error handling guide

All Terrain endpoints are documented with request/response schemas and examples.

### How do I migrate from Tapis v2 to Tapis v3?

Tapis v3 introduces breaking changes. Follow the [Tapis v2 to v3 Migration Guide](../services/api/tapis-v2-v3-migration.md) which covers:

- API endpoint changes
- Authentication updates
- Request/response format changes
- Deprecated features
- New capabilities

Plan for code changes in your integrations.

### What are the API rate limits?

Current rate limiting (subject to change):

- **100 requests/minute** per user for authenticated endpoints
- **10 requests/minute** for unauthenticated endpoints
- **Burst limit**: 150 requests in 10 seconds

If you exceed limits, you'll receive `HTTP 429 Too Many Requests`. Implement exponential backoff in your client code.

For higher limits, contact CyVerse support with your use case.

### How do I upload large files via the API?

For large files (>100MB), use the chunked upload endpoint:

```python
import requests

def upload_large_file(access_token, local_file, dest_path, chunk_size=10*1024*1024):
    """Upload large file in chunks."""
    # 1. Initialize upload session
    init_response = requests.post(
        "https://de.cyverse.org/terrain/secured/fileio/upload-init",
        headers={"Authorization": f"Bearer {access_token}"},
        json={"dest": dest_path, "filename": "largefile.dat"}
    )
    upload_id = init_response.json()["upload_id"]

    # 2. Upload chunks
    with open(local_file, "rb") as f:
        chunk_num = 0
        while True:
            chunk = f.read(chunk_size)
            if not chunk:
                break
            requests.post(
                f"https://de.cyverse.org/terrain/secured/fileio/upload-chunk/{upload_id}",
                headers={"Authorization": f"Bearer {access_token}"},
                files={"chunk": chunk},
                data={"chunk_number": chunk_num}
            )
            chunk_num += 1

    # 3. Finalize upload
    requests.post(
        f"https://de.cyverse.org/terrain/secured/fileio/upload-complete/{upload_id}",
        headers={"Authorization": f"Bearer {access_token}"}
    )
```

See [File I/O endpoints](../services/api/endpoints/fileio.md) for details.

### How do I receive notifications when my job completes?

Enable notifications when submitting a job:

```python
payload = {
    "app_id": "app-uuid",
    "name": "My Job",
    "notify": True,  # Enable notifications
    "config": {...}
}
```

You'll receive notifications via:
- In-app notifications in Discovery Environment
- Email (if configured in user preferences)
- Webhook callbacks (if configured)

See [Notifications API](../services/api/endpoints/notifications.md) and [Callbacks API](../services/api/endpoints/callbacks.md).

### Can I run the API locally for development?

Yes! Clone the [terrain repository](https://github.com/cyverse-de/terrain){target=_blank} and other services:

```bash
# Clone terrain
git clone https://github.com/cyverse-de/terrain.git
cd terrain

# Install dependencies (Clojure/Leiningen)
lein deps

# Configure local settings
cp config/terrain.properties.example config/terrain.properties
# Edit config to point to local databases

# Run locally
lein run
```

See repository README files for service-specific setup instructions.

---

## Troubleshooting

### Why can't users log in to the Discovery Environment?

**Common authentication issues:**

1. **KeyCloak service down** - Check `kubectl get pods -n keycloak`
2. **Database connection failure** - KeyCloak can't reach its database
3. **LDAP sync issues** - If using LDAP, verify connection to LDAP server
4. **Browser cache/cookies** - Have user clear browser cache and try incognito
5. **CILogon integration broken** - Check CILogon service status
6. **Certificate issues** - Verify TLS certificates are valid

**Debug steps:**
```bash
# Check KeyCloak logs
kubectl logs -n keycloak deployment/keycloak

# Test database connection
kubectl exec -it -n keycloak deployment/keycloak -- psql -h db-host -U keycloak

# Verify LDAP connectivity
kubectl exec -it -n keycloak deployment/keycloak -- ldapsearch -x -H ldap://ldap-server
```

### Why is the Data Store slow?

**Performance bottlenecks:**

1. **iRODS server overload** - Check CPU/memory on iRODS servers
2. **Network congestion** - Verify network bandwidth between nodes
3. **Slow storage backend** - Check underlying storage performance (NFS, SAN)
4. **Database issues** - iRODS catalog queries slow
5. **Too many concurrent users** - Consider scaling iRODS servers

**Monitoring:**
```bash
# Check iRODS server status
ils -L  # List with verbose output, observe latency

# Monitor iRODS resource usage
kubectl top pods -n irods

# Check network performance
iperf3 -c irods-server
```

See [Data Store documentation](../services/ds.md) for optimization tips.

### Why are jobs stuck in "Submitted" status?

**Job execution issues:**

1. **HTCondor/K8s not processing jobs** - Check job execution platform
2. **Insufficient resources** - No available compute nodes
3. **Input staging failures** - Data transfer from Data Store failed
4. **App configuration errors** - Invalid app definition
5. **Permission issues** - User lacks access to required resources

**Debug steps:**
```bash
# Check HTCondor queue (for batch jobs)
condor_q -analyze <job-id>

# Check Kubernetes pods (for VICE jobs)
kubectl get pods -n vice-apps

# Check analyses service logs
kubectl logs -n de deployment/analyses
```

### Jobs are failing with "Out of Memory" errors

**Memory issues:**

1. **Insufficient container limits** - Increase memory limit in app definition
2. **User requested too little memory** - Guide users to request appropriate resources
3. **Node exhaustion** - Add more Kubernetes nodes or increase node sizes
4. **Memory leaks in tool** - Issue with the containerized tool itself

**Solutions:**
- Increase memory limits: Edit app in DE Apps interface
- Add resource quotas: Define default/max memory for different app categories
- Scale cluster: Add more compute nodes

See [Kubernetes resources documentation](../deployments/k8s-resources.md).

### ElasticSearch is using too much disk space

**ElasticSearch index management:**

```bash
# Check index sizes
curl -X GET "localhost:9200/_cat/indices?v&s=store.size:desc"

# Delete old indices
curl -X DELETE "localhost:9200/old-index-name"

# Set up index lifecycle management (ILM)
curl -X PUT "localhost:9200/_ilm/policy/cleanup-policy" -H 'Content-Type: application/json' -d'
{
  "policy": {
    "phases": {
      "delete": {
        "min_age": "30d",
        "actions": {"delete": {}}
      }
    }
  }
}'
```

See [ElasticSearch deployment documentation](../deployments/elasticsearch.md).

---

## Data Management

### How do I share data with collaborators?

**Data sharing options:**

1. **Share within CyVerse** - Give users/groups read or write permissions
   ```bash
   # Via Data Store API
   POST /terrain/secured/filesystem/share
   {
     "paths": ["/iplant/home/user/folder"],
     "permissions": ["read"],
     "users": ["collaborator1", "collaborator2"]
   }
   ```

2. **Public links** - Create anonymous access tickets
   - In DE: Right-click file/folder → Share → Create Public Link

3. **DOI publication** - Publish to Data Commons with permanent identifier

See [Sharing API](../services/api/endpoints/filesystem/sharing.md) and [Data Store Guide](ds.md).

### How do I search for data across the entire Data Store?

Use the search API:

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X POST https://de.cyverse.org/terrain/secured/filesystem/search \
  -H "Content-Type: application/json" \
  -d '{"query": "genomics", "type": "file"}'
```

Search supports:
- Full-text search in filenames and metadata
- Filters by file type, date, size
- User-defined metadata (AVU) searches

See [Search API documentation](../services/api/endpoints/filesystem/search.md).

### How do I attach metadata to data files?

Add **Attribute-Value-Unit (AVU)** metadata:

```python
# Add metadata via API
requests.post(
    "https://de.cyverse.org/terrain/secured/filesystem/metadata",
    headers={"Authorization": f"Bearer {token}"},
    json={
        "path": "/iplant/home/user/data.csv",
        "metadata": [
            {"attr": "experiment_date", "value": "2024-01-15", "unit": ""},
            {"attr": "sample_size", "value": "1000", "unit": "samples"}
        ]
    }
)
```

Or use the DE interface: Right-click file → Metadata → Add

See [Metadata API](../services/api/endpoints/filesystem/metadata.md).

### How do I recover deleted data?

Deleted files go to trash and can be restored within **30 days**:

**Via Discovery Environment:**
1. Navigate to Trash folder
2. Select files/folders to restore
3. Click "Restore"

**Via API:**
```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X POST https://de.cyverse.org/terrain/secured/filesystem/restore \
  -H "Content-Type: application/json" \
  -d '{"paths": ["/iplant/trash/user/deleted-folder"]}'
```

After 30 days, files are permanently deleted. Implement backups for critical data.

See [Restore API](../services/api/endpoints/filesystem/restore.md).

### What's the maximum file size I can store?

**File size limits:**

- **Single file**: No hard limit (tested up to 100+ GB)
- **Upload via web UI**: 2 GB recommended (browser limitations)
- **Upload via API/CLI**: No limit
- **iCommands (iRODS CLI)**: Recommended for files >2 GB

For very large files (>100 GB), use:
- **iCommands**: `iput` for direct iRODS uploads
- **GoCommands**: CyVerse's modern iRODS CLI
- **Globus**: For scheduled large transfers

See [Data Store documentation](../services/ds.md).

---

## Still Have Questions?

- **Search this documentation** - Use the search bar at the top
- **Check service-specific docs** - See [Deployment Guide](../deployments/deployment_overview.md), [API Reference](../services/api_overview.md), or [Admin Guides](devops.md)
- **GitHub Issues** - Report issues or ask questions at [github.com/cyverse-de](https://github.com/cyverse-de){target=_blank}
- **CyVerse Support** - Contact support at [cyverse.org/help](https://cyverse.org/help){target=_blank}
