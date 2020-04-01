# How to install frp client in Kubernetes

frp is a common tool that used to set up HTTP or TCP tunnel between 2 networks. A usual use case is set up a tunnel between home server and public server so that you can access your internal service on the Internet.

This page assumes that:  
1. Your frp server is openning at "frps.example.com"  
2. You have a Kubernetes Service names "demo-service", you can access it via "demo-service:8080" inside the cluster

### Create frpc.ini

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

### Create Kubernetes Secret

```text
kubectl create secret generic frpc-config --from-file=./frpc.ini
```

### Deploy frpc Container

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
          image: snowdreamtech/frpc
          volumeMounts:
            - name: config
              mountPath: "/etc/frp"
              readOnly: true
      volumes:
        - name: config
          secret:
            secretName: frpc-config
```



1. snowdreamtech/frpc loads frpc configuration file in `/etc/frp/frpc.ini` by default
2. We save frpc.ini as a secret becase generally we put authentication token in it



