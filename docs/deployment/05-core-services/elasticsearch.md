---
type: Deployment Procedure
title: "Elasticsearch (legacy)"
description: "The legacy Elasticsearch stateful set and index mappings, superseded by OpenSearch in new deployments."
tags: [deployment, core-services, elasticsearch, search]
status: deprecated
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---
# Elasticsearch

!!! warning "Superseded by OpenSearch"

    New deployments install [OpenSearch](./opensearch.md) instead. This document
    is kept for the clusters still running Elasticsearch — the index mappings
    below are Elasticsearch 6 era and use `_parent` and multiple mapping types
    per index, neither of which exists in current Elasticsearch or OpenSearch.
    Do not copy them into a new deployment.

Deploying the Elasticsearch stateful set on Kubernetes.

!!! success "Prerequisites"

    * A checkout of the [cluster resources](../04-kubernetes/resources.md)
      repository; the paths below are relative to its root.
    * A storage class for the stateful set; see
      [storage](../04-kubernetes/storage.md).
    * On a small cluster, lower the replica count and heap in
      `resources/addons/elasticsearch/elasticsearch.yml`:

        ```yaml
          replicas: 2
            resources:
              requests:
                memory: "4Gi"
              limits:
                memory: "4Gi"
              - name: ES_JAVA_OPTS
                value: "-Xms2g -Xmx2g"
        ```

### Deploy

Substitute the namespace the DE services run in:

```bash
kubectl apply -n <NAMESPACE> -f resources/addons/elasticsearch/elasticsearch.yml
```

## Indexing

### Prerequisites

Save below json to a file `settings.json`, we will use this file to index our elasticsearch.

```json
{
    "mappings": {
      "file": {
        "properties": {
          "creator": {
            "type": "keyword"
          },
          "dateCreated": {
            "type": "date"
          },
          "dateModified": {
            "type": "date"
          },
          "fileSize": {
            "type": "long"
          },
          "fileType": {
            "type": "keyword"
          },
          "id": {
            "type": "keyword"
          },
          "label": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword"
              }
            },
            "analyzer": "irods_entity"
          },
          "metadata": {
            "type": "nested",
            "properties": {
              "attribute": {
                "type": "text",
                "analyzer": "irods_entity"
              },
              "unit": {
                "type": "keyword"
              },
              "value": {
                "type": "text"
              }
            }
          },
          "path": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword"
              }
            },
            "analyzer": "irods_path"
          },
          "userPermissions": {
            "type": "nested",
            "properties": {
              "permission": {
                "type": "keyword"
              },
              "user": {
                "type": "keyword"
              }
            }
          }
        }
      },
      "tag": {
        "properties": {
          "creator": {
            "type": "keyword"
          },
          "dateCreated": {
            "type": "date"
          },
          "dateModified": {
            "type": "date"
          },
          "description": {
            "type": "text"
          },
          "id": {
            "type": "keyword"
          },
          "targets": {
            "type": "nested",
            "properties": {
              "id": {
                "type": "keyword"
              },
              "type": {
                "type": "keyword"
              }
            }
          },
          "value": {
            "type": "text",
            "analyzer": "tag_value"
          }
        }
      },
      "file_metadata": {
        "_parent": {
          "type": "file"
        },
        "_routing": {
          "required": true
        },
        "properties": {
          "id": {
            "type": "keyword"
          },
          "metadata": {
            "type": "nested",
            "properties": {
              "attribute": {
                "type": "text",
                "analyzer": "irods_entity"
              },
              "unit": {
                "type": "keyword"
              },
              "value": {
                "type": "text"
              }
            }
          }
        }
      },
      "folder": {
        "properties": {
          "creator": {
            "type": "keyword"
          },
          "dateCreated": {
            "type": "date"
          },
          "dateModified": {
            "type": "date"
          },
          "fileSize": {
            "type": "long"
          },
          "fileType": {
            "type": "keyword"
          },
          "id": {
            "type": "keyword"
          },
          "label": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword"
              }
            },
            "analyzer": "irods_entity"
          },
          "metadata": {
            "type": "nested",
            "properties": {
              "attribute": {
                "type": "text",
                "analyzer": "irods_entity"
              },
              "unit": {
                "type": "keyword"
              },
              "value": {
                "type": "text"
              }
            }
          },
          "path": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword"
              }
            },
            "analyzer": "irods_path"
          },
          "userPermissions": {
            "type": "nested",
            "properties": {
              "permission": {
                "type": "keyword"
              },
              "user": {
                "type": "keyword"
              }
            }
          }
        }
      },
      "folder_metadata": {
        "_parent": {
          "type": "folder"
        },
        "_routing": {
          "required": true
        },
        "properties": {
          "id": {
            "type": "keyword"
          },
          "metadata": {
            "type": "nested",
            "properties": {
              "attribute": {
                "type": "text",
                "analyzer": "irods_entity"
              },
              "unit": {
                "type": "keyword"
              },
              "value": {
                "type": "text"
              }
            }
          }
        }
      }
    },
    "settings": {
      "index": {
        "mapper": {
          "dynamic": "false"
        },
        "analysis": {
          "analyzer": {
            "irods_entity": {
              "filter": [
                "asciifolding",
                "lowercase"
              ],
              "type": "custom",
              "tokenizer": "irods_entity"
            },
            "irods_path": {
              "type": "custom",
              "tokenizer": "irods_path"
            },
            "tag_value": {
              "filter": [
                "asciifolding",
                "lowercase"
              ],
              "type": "custom",
              "tokenizer": "keyword"
            }
          },
          "tokenizer": {
            "irods_entity": {
              "type": "keyword",
              "buffer_size": "2700"
            },
            "irods_path": {
              "type": "path_hierarchy",
              "buffer_size": "2700"
            }
          }
        },
        "number_of_replicas": "1"
      }
    }
  }
```

### Index

For indexing we will run a container inside your **namespace** where the elasticsearch is running, and copy the `settings.json` file inside this container and run the following commands:

```bash
# start a utility pod in the namespace the search cluster runs in
kubectl run --namespace=<NAMESPACE> --rm utils -it --image arunvelsriram/utils bash

# then, from a second terminal, copy the mappings into that pod
kubectl -n <NAMESPACE> cp settings.json utils:/home/utils
```

#### run indexing

Inside the running container shell run this command to add the indexes.

```bash
# check elasticsearch health
curl -XGET "http://elasticsearch:9200/_cluster/health?pretty"

# run indexing from file
curl -sX PUT "http://elasticsearch:9200/data" -d @settings.json
```

#### (optional) delete current indexes

```bash
# delete data indexes
curl -sX DELETE "http://elasticsearch:9200/data"

# delete everything
curl -sX DELETE "http://elasticsearch:9200/*"
```

#### restart related services

```bash
kubectl rollout restart deployment infosquito2 search -n <NAMESPACE>
```

Restarting the two consumers is what is needed; the stateful set itself does not
have to be restarted for a mapping change on a new index.

### Related

* [OpenSearch](./opensearch.md) — what new deployments use instead
* [Reindex search](../01-foundation/rabbitmq.md#operations-reindex-search)
