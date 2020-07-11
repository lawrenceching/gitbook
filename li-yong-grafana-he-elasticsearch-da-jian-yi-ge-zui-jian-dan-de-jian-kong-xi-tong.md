# 利用 Grafana 和 ElasticSearch 搭建一个最简单的监控系统

本文假设你已经对 Grafana 和 ElasticSearch 有初步的了解。同样假设你已经安装好了两者，故不再赘述具体安装教程。

### 创建 Index

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

### 提交数据

```text
curl -X POST 'localhost:9200/simple/_doc?pretty'  -H 'Content-Type: application/json' -d'
{
    "value": 1045,
    "@timestamp": 1593267490678
}
```

### 新建数据源

![](.gitbook/assets/image%20%2819%29.png)

### 查询数据

![](.gitbook/assets/image%20%2821%29.png)

