# jaeger

Jaeger: open source, end-to-end distributed tracing

!!! success "Prerequisites"


## Deploy

The deployment manifests are in [k8s-resources](k8s-resources.md).

**Create namespace**
```bash
kubectl create ns jaeger
```


**(optional) changing the env**

Modify files to change the namespace:

* `resources/addons/jaeger/collector.yaml`
* `resources/addons/jaeger/query.yaml`
* `resources/addons/jaeger/rollover-cron.yaml`

```diff
- "http://elasticsearch.prod:9200"
+ "http://elasticsearch.discover:9200"
```

**Apply manifests**
```bash
kubectl apply -f resources/addons/jaeger/rollover-cron.yaml -n jaeger
kubectl apply -f resources/addons/jaeger/query.yaml -n jaeger
kubectl apply -f resources/addons/jaeger/collector.yaml -n jaeger
```

