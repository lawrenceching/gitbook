# How to install frp client in Kubernetes

本文假设：

* 你已经成功部署 frps。 假设 frps 部署在 frps.example.com:7000
* 你已经部署了可用的 Kubernetes 集群
* 你有一个叫 "demo-service" 的 Service

### 创建 frpc.ini

这里 `demo-service:8080` 为 k8s 集群内部的 DNS 地址。

```text
[common]
server_addr = frps.example.com
server_port = 7000

[demo-service]
type = tcp
local_ip = demo-service
local_port = 8080
remote_port = 8080
```

### 创建 Secret

```text
kubectl create secret generic frpc-config --from-file=./frpc.ini
```

### 部署 frpc 容器

```text
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frpc
  labels:
    app: frpc
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frpc
  template:
    metadata:
      labels:
        app: frpc
    spec:
      containers:
        - name: frpc
          image: dockerhub.azk8s.cn/snowdreamtech/frpc
          volumeMounts:
            - name: config
              mountPath: "/etc/frp"
              readOnly: true
      volumes:
        - name: config
          secret:
            secretName: frpc-config
```

1. snowdreamtech/frpc 的默认配置文件路径为 `/etc/frp/frpc.ini`
2. frpc.ini 以 k8s Secret 的形式储存在集群内。一是修改 Secret 比较方便。二是通常来说配置文件里有 authentcation token 或者 OIDC 配置信息，以 Secret 形式保存更为合适。
3. `dockerhub.azk8s.cn/snowdreamtech/frpc` 为 `snowdreamtech/frpc` 的 Azure 镜像地址。

