# Elasticsearch

Deploying the `statefulsets` Elasticsearch cluster on kubernetes.


!!! success "Prerequisites"

    * clone the repo 
    
    * see also [k8s-resources](k8s-resources.md)

    * `cd /k8s-resources`

    * In case you have limited resources change these:

    `cd /k8s-resources/resources/addons/elasticsearch/elasticsearch.yml`

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

```bash
# deploy ES on prod env
kubectl apply -n prod -f resources/addons/elasticsearch/elasticsearch.yml

## deploy ES on discover env
# kubectl apply -n discover -f resources/addons/elasticsearch/elasticsearch.yml
```

## Indexing

### Preq

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
## run container if es in prod namespace
kubectl run --namespace=prod --rm utils -it --image arunvelsriram/utils bash

## run container if es in discover namespace
# kubectl run --namespace=discover --rm utils -it --image arunvelsriram/utils bash

######### PS open a new terminal#########
## copy the settings.json if PROD
kubectl -n prod cp settings.json utils:/home/utils

## copy the settings.json if Discover
# kubectl -n discover cp settings.json utils:/home/utils

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
# kubectl rollout restart statefulset elasticsearch -n prod # not sure
kubectl rollout restart deployment infosquito2 search -n <NAMESPACE>
```
