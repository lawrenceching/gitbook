---
description: >-
  This page demonstrates the simplest implementation that use ElasticSearch and
  Grafana to monitor metrics
---

# Monitoring, ElasticSearch and Grafana

### Create Index in ElasticSearch

```text
curl -X PUT "localhost:9200/simple?pretty" -H 'Content-Type: application/json' -d'
{ "mappings": {
    "properties": {
        "@timestamp": {
            "type": "date"
        },
        "value": {
            "type": "long"
        }
    }
}
}
'
```

### Submit Metric to ElasticSearch

```text
curl -X POST 'localhost:9200/simple/_doc?pretty'  -H 'Content-Type: application/json' -d'
{
    "value": 1045,
    "@timestamp": 1593267490678
}
```

### Add ElasticSearch as Data Source in Grafana

![](.gitbook/assets/image%20%2817%29.png)

### Query Metrics in Grafana

![](.gitbook/assets/image%20%2818%29.png)

