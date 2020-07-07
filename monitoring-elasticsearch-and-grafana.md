---
description: >-
  This page demostrates a simple implementation that use ElasticSearch and
  Grafana to monitor application metrics
---

# Monitoring, ElasticSearch and Grafana

## Create Index in ElasticSearch

```text
curl -X PUT "localhost:9200/simple?pretty"
```

Submit Metric to ElasticSearch

```text
curl -X POST 'localhost:9200/simple/_doc?pretty'  -H 'Content-Type: application/json' -d'
{
    "count": 1045,
    "@timestamp": 1593267490678
}
```



