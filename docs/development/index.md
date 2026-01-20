# Developer Guide

Welcome to the CyVerse Developer Guide. This documentation is for developers who want to:

- Integrate applications with CyVerse APIs
- Contribute code to the CyVerse platform
- Build custom tools and services on top of CyVerse infrastructure
- Understand CyVerse architecture for research or collaboration

## Who This Guide Is For

**API Integrators**: You want to connect your application to CyVerse's Data Store, submit computational jobs, or leverage CyVerse authentication.

**Platform Contributors**: You want to contribute code, fix bugs, or add features to CyVerse's open-source codebase.

**Tool Developers**: You're packaging scientific software to run in the Discovery Environment or VICE.

**Researchers & Data Scientists**: You want to automate workflows, build custom interfaces, or extend CyVerse functionality for your research needs.

---

## Getting Started

### Development Environment Setup

#### Required Tools

Before you begin, install these tools:

```bash
# Version control
git --version  # Git 2.30+

# Container tools
docker --version  # Docker 20.10+
docker-compose --version  # Docker Compose 1.29+

# Kubernetes tools (for platform development)
kubectl version  # kubectl 1.23+
helm version  # Helm 3.8+

# Programming languages (depending on services)
python --version  # Python 3.9+
go version  # Go 1.19+ (for DE services)
node --version  # Node.js 16+ (for UI development)
```

#### Get Access

1. **CyVerse Account**: Create a free account at [https://user.cyverse.org](https://user.cyverse.org){target=_blank}
2. **GitHub Access**: Most CyVerse code is at [github.com/cyverse-de](https://github.com/cyverse-de){target=_blank}
3. **API Credentials**: Generate API tokens through the Discovery Environment or User Portal
4. **Development Environment**: Set up a test environment or use the public CyVerse production instance

#### Development Accounts

For testing and development:

- **Production**: `https://de.cyverse.org` - Use for API testing with real data (be cautious!)
- **Development/Staging**: Contact CyVerse team for access to dev environments
- **Local Development**: Set up local services using Docker Compose (see repository README files)

---

## CyVerse Architecture Overview

Understanding CyVerse's architecture will help you integrate effectively or contribute meaningfully.

### System Design

CyVerse follows a **microservices architecture** with services communicating via REST APIs and message queues:

```
┌─────────────────────────────────────────────────────────────┐
│                     Users & Applications                      │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                     Frontend Layer                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ Discovery    │  │ User Portal  │  │ Custom Apps  │      │
│  │ Environment  │  │              │  │ (Your Code)  │      │
│  │ (Sonora UI)  │  │              │  │              │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                    API Gateway Layer                         │
│  ┌───────────────────────────────────────────────────────┐  │
│  │              Terrain API (Aggregator)                  │  │
│  │         https://de.cyverse.org/terrain/docs            │  │
│  └───────────────────────────────────────────────────────┘  │
└───────────────────────┬─────────────────────────────────────┘
                        │
        ┌───────────────┼───────────────┐
        │               │               │
        ▼               ▼               ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ Microservices Layer                              │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐          │
│ │ apps     │ │ data-info│ │ analyses │          │
│ │          │ │          │ │          │          │
│ └──────────┘ └──────────┘ └──────────┘          │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐          │
│ │ metadata │ │ notif-   │ │ permiss- │          │
│ │          │ │ ications │ │ ions     │          │
│ └──────────┘ └──────────┘ └──────────┘          │
└──────────────────────────────────────────────────┘
        │               │               │
        └───────────────┼───────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                  Infrastructure Layer                        │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │ PostgreSQL  │ │ RabbitMQ    │ │ iRODS       │           │
│  │ (Databases) │ │ (Messages)  │ │ (Storage)   │           │
│  └─────────────┘ └─────────────┘ └─────────────┘           │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │ ElasticSrch │ │ KeyCloak    │ │ Kubernetes  │           │
│  │ (Search)    │ │ (Auth)      │ │ (Orchestr.) │           │
│  └─────────────┘ └─────────────┘ └─────────────┘           │
└─────────────────────────────────────────────────────────────┘
```

### Key Components

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Terrain** | Clojure | API gateway aggregating all DE services |
| **apps** | Go | App/tool metadata and management |
| **data-info** | Clojure | Data Store filesystem operations |
| **analyses** | Go | Job submission and monitoring |
| **metadata** | Go | User-defined metadata (AVUs) |
| **notifications** | Go | User notification system |
| **permissions** | Go | Fine-grained permission management |
| **Sonora** | React/TypeScript | Modern DE web UI |
| **iRODS** | C++/Python | Data storage backend |
| **KeyCloak** | Java | Authentication and identity |

### Service Interactions

**Example: Submitting a Job**

1. User clicks "Run" in Sonora UI
2. Sonora calls Terrain API: `POST /terrain/analyses`
3. Terrain validates request and calls `analyses` service
4. `analyses` service:
   - Validates app exists (via `apps` service)
   - Checks user permissions (via `permissions` service)
   - Stages input data (via `data-info` service)
   - Submits job to HTCondor or Kubernetes
   - Sends notification (via `notifications` service)
   - Publishes events to RabbitMQ
5. Job status updates flow back through the same chain

---

## API Integration

### Authentication

CyVerse uses **OAUTH 2.0** via KeyCloak for authentication.

#### Getting an Access Token

**Method 1: Interactive (Browser-based)**

Use OAUTH 2.0 authorization code flow:

```python
import requests

# Step 1: Direct user to authorization URL
auth_url = "https://kc.cyverse.org/auth/realms/CyVerse/protocol/openid-connect/auth"
params = {
    "client_id": "your-client-id",
    "redirect_uri": "https://yourapp.com/callback",
    "response_type": "code",
    "scope": "openid"
}
# User visits this URL and authorizes

# Step 2: Exchange authorization code for token
token_url = "https://kc.cyverse.org/auth/realms/CyVerse/protocol/openid-connect/token"
token_response = requests.post(token_url, data={
    "grant_type": "authorization_code",
    "code": "authorization-code-from-callback",
    "redirect_uri": "https://yourapp.com/callback",
    "client_id": "your-client-id",
    "client_secret": "your-client-secret"
})
access_token = token_response.json()["access_token"]
```

**Method 2: Command-line (for scripts)**

For automated scripts, use resource owner password credentials flow (requires user consent):

```bash
#!/bin/bash
# Get access token for scripting

USERNAME="your-username"
PASSWORD="your-password"
CLIENT_ID="de-client"

TOKEN=$(curl -s -X POST \
  https://kc.cyverse.org/auth/realms/CyVerse/protocol/openid-connect/token \
  -d "grant_type=password" \
  -d "client_id=${CLIENT_ID}" \
  -d "username=${USERNAME}" \
  -d "password=${PASSWORD}" \
  | jq -r '.access_token')

echo $TOKEN
```

#### Using the Access Token

Include the token in the `Authorization` header:

```bash
curl -H "Authorization: Bearer ${TOKEN}" \
  https://de.cyverse.org/terrain/filesystem/list
```

### Making API Calls

#### Example: List Files

```python
import requests

def list_directory(access_token, path):
    """List files in a Data Store directory."""
    url = f"https://de.cyverse.org/terrain/secured/filesystem/paged-directory"
    headers = {
        "Authorization": f"Bearer {access_token}",
        "Content-Type": "application/json"
    }
    params = {
        "path": path,
        "limit": 100,
        "offset": 0
    }

    response = requests.get(url, headers=headers, params=params)
    response.raise_for_status()
    return response.json()

# Usage
files = list_directory(access_token, "/iplant/home/username")
for item in files["files"]:
    print(f"{item['label']} ({item['file-size']} bytes)")
```

#### Example: Submit a Job

```python
def submit_analysis(access_token, app_id, input_files, parameters):
    """Submit a job to run an analysis."""
    url = "https://de.cyverse.org/terrain/analyses"
    headers = {
        "Authorization": f"Bearer {access_token}",
        "Content-Type": "application/json"
    }
    payload = {
        "app_id": app_id,
        "name": "My Analysis",
        "description": "Running analysis via API",
        "notify": True,
        "config": {
            "input1": input_files,
            "param1": parameters
        }
    }

    response = requests.post(url, headers=headers, json=payload)
    response.raise_for_status()
    return response.json()

# Usage
result = submit_analysis(
    access_token,
    app_id="12345678-1234-1234-1234-123456789abc",
    input_files=["/iplant/home/username/data.txt"],
    parameters={"threshold": 0.05}
)
print(f"Job submitted: {result['id']}")
```

#### Example: Upload a File

```python
def upload_file(access_token, local_file, dest_path):
    """Upload a file to the Data Store."""
    url = "https://de.cyverse.org/terrain/secured/fileio/upload"
    headers = {
        "Authorization": f"Bearer {access_token}"
    }
    files = {
        "file": open(local_file, "rb")
    }
    data = {
        "dest": dest_path
    }

    response = requests.post(url, headers=headers, files=files, data=data)
    response.raise_for_status()
    return response.json()

# Usage
upload_file(access_token, "local-data.csv", "/iplant/home/username/uploads/")
```

### API Resources

- **[Live API Documentation](https://de.cyverse.org/terrain/docs/){target=_blank}** - Interactive Swagger UI
- **[Terrain Endpoint Reference](../services/api/endpoint-index.md)** - Comprehensive endpoint list
- **[API Error Codes](../services/api/errors.md)** - Error handling guide
- **[Filesystem API](../services/api/endpoints/filesystem/directory-listing.md)** - Data Store operations
- **[Tapis Migration Guide](../services/api/tapis-v2-v3-migration.md)** - Upgrading from Tapis v2 to v3

---

## Building Custom Applications

### Best Practices

1. **Use Access Tokens Securely**
   - Never commit tokens to version control
   - Use environment variables or secure vaults
   - Implement token refresh logic

2. **Handle Rate Limits**
   - Implement exponential backoff for retries
   - Cache responses when appropriate
   - Use bulk operations when available

3. **Error Handling**
   - Check HTTP status codes
   - Parse error responses for details
   - Provide user-friendly error messages

4. **Respect User Privacy**
   - Only access data the user has authorized
   - Follow CyVerse data policies
   - Implement proper permission checks

### Example: Python SDK Wrapper

Create a simple SDK wrapper for common operations:

```python
# cyverse_sdk.py
import requests
from typing import List, Dict

class CyVerseClient:
    def __init__(self, access_token: str, base_url: str = "https://de.cyverse.org/terrain"):
        self.access_token = access_token
        self.base_url = base_url
        self.session = requests.Session()
        self.session.headers.update({
            "Authorization": f"Bearer {access_token}",
            "Content-Type": "application/json"
        })

    def list_files(self, path: str, limit: int = 100) -> List[Dict]:
        """List files in a directory."""
        url = f"{self.base_url}/secured/filesystem/paged-directory"
        response = self.session.get(url, params={"path": path, "limit": limit})
        response.raise_for_status()
        return response.json()["files"]

    def search_data(self, query: str) -> List[Dict]:
        """Search for data in the Data Store."""
        url = f"{self.base_url}/secured/filesystem/search"
        response = self.session.post(url, json={"query": query})
        response.raise_for_status()
        return response.json()["files"]

    def get_analyses(self, limit: int = 10) -> List[Dict]:
        """Get user's recent analyses."""
        url = f"{self.base_url}/analyses"
        response = self.session.get(url, params={"limit": limit})
        response.raise_for_status()
        return response.json()["analyses"]

# Usage
client = CyVerseClient(access_token="your-token")
files = client.list_files("/iplant/home/username")
```

---

## Contributing to CyVerse

### GitHub Workflow

CyVerse development follows standard Git/GitHub workflows:

1. **Fork the Repository**
   ```bash
   # Navigate to the service repository on GitHub and click "Fork"
   git clone https://github.com/YOUR-USERNAME/repo-name.git
   cd repo-name
   git remote add upstream https://github.com/cyverse-de/repo-name.git
   ```

2. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make Changes and Commit**
   ```bash
   git add .
   git commit -m "Add feature: description of changes"
   ```

4. **Push and Create Pull Request**
   ```bash
   git push origin feature/your-feature-name
   # Then create a Pull Request on GitHub
   ```

### Code Review Process

1. Submit a Pull Request with:
   - Clear description of changes
   - Reference to any related issues
   - Screenshots for UI changes
   - Test results

2. Automated checks will run:
   - Unit tests
   - Linting/formatting checks
   - Security scans

3. Maintainers will review your code:
   - Respond to feedback promptly
   - Make requested changes
   - Engage in technical discussions

4. Once approved, maintainers will merge your PR

### Testing Requirements

All contributions should include appropriate tests:

**Go Services:**
```bash
# Run unit tests
go test ./...

# Run with coverage
go test -cover ./...
```

**Clojure Services:**
```bash
# Run tests with Leiningen
lein test
```

**React/TypeScript UI:**
```bash
# Run Jest tests
npm test

# Run with coverage
npm test -- --coverage
```

### Development Standards

- **Code Style**: Follow existing code style in the repository
- **Documentation**: Update README and inline comments for significant changes
- **Commit Messages**: Use clear, descriptive commit messages
- **Breaking Changes**: Discuss breaking changes with maintainers before implementing

---

## CyVerse Repositories

### Core Services

| Repository | Language | Description |
|------------|----------|-------------|
| [terrain](https://github.com/cyverse-de/terrain){target=_blank} | Clojure | Main API gateway |
| [apps](https://github.com/cyverse-de/apps){target=_blank} | Go | App metadata service |
| [data-info](https://github.com/cyverse-de/data-info){target=_blank} | Clojure | Data Store operations |
| [analyses](https://github.com/cyverse-de/analyses){target=_blank} | Go | Job management |
| [sonora](https://github.com/cyverse-de/sonora){target=_blank} | React/TS | Modern DE UI |

### Infrastructure

| Repository | Language | Description |
|------------|----------|-------------|
| [deployments](https://github.com/cyverse-de/deployments){target=_blank} | YAML/HCL | Kubernetes manifests |
| [ansible](https://github.com/cyverse/ansible){target=_blank} | Ansible | Configuration management |

### Tools & Utilities

| Repository | Language | Description |
|------------|----------|-------------|
| [gocommands](https://github.com/cyverse/gocommands){target=_blank} | Go | iRODS CLI tools |
| [irods-csi-driver](https://github.com/cyverse/irods-csi-driver){target=_blank} | Go | Kubernetes iRODS integration |

Browse the full list at [github.com/cyverse-de](https://github.com/cyverse-de){target=_blank}

---

## Community & Support

### Get Help

- **Documentation**: You're reading it! Search this site for answers
- **GitHub Issues**: Report bugs or request features in the relevant repository
- **CyVerse Support**: [support.cyverse.org](https://cyverse.org/help){target=_blank}

### Stay Updated

- **GitHub**: Watch repositories for updates
- **CyVerse Blog**: [cyverse.org/blog](https://cyverse.org){target=_blank}

### Contributing Beyond Code

Not a developer? You can still contribute:

- Report bugs you encounter
- Suggest feature improvements
- Write documentation
- Answer questions in forums
- Share your CyVerse integration as an example

---

## Additional Resources

- **[API Overview](../services/api_overview.md)** - Detailed Terrain API documentation
- **[System Architecture](../services/services_overview.md)** - Deep dive into CyVerse design
- **[Deployment Guide](../deployments/deployment_overview.md)** - For platform operators
- **[Getting Started](../services/getting_started.md)** - Prerequisites and initial setup

**Ready to build?** Start with the [API Endpoint Index](../services/api/endpoint-index.md) to explore available functionality, or dive into the [live API documentation](https://de.cyverse.org/terrain/docs/){target=_blank} for interactive testing.
