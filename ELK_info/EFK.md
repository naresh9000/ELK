**centralized logging**

Prerequisites
Amazon EKS Cluster: Ensure you have an operational Amazon EKS cluster with kubectl configured to communicate with the cluster.
Elasticsearch Cluster: Set up an Elasticsearch cluster that will store and index the logs from your EKS pods.

1) 
Deploy Elasticsearch on EKS
```
helm repo add elastic https://Helm.elastic.co
helm install elasticsearch elastic/elasticsearch

```

2) 
Deploy Fluentd as a Sidecar

fluentd-config.yaml
```

apiVersion: v1
kind: ConfigMap
metadata:
  name: fluentd-config
data:
  fluent.conf: |
    <source>
      @type forward
      port 24224
      bind 0.0.0.0
    </source>
    <match **>
      @type elasticsearch
      hosts elasticsearch-master:9200
      logstash_format true
      include_tag_key true
      type_name "_doc"
      flush_interval 1s
    </match>
```


**How do elasticsearch is configured to get metrics from fluentd**


