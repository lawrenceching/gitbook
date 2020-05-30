---
description: 如果你遇到 Pod 无法解析 DNS，coredns 日志显示 "no route to host"，尝试 flush iptables 看能否解决问题。
---

# 快速解决  Kubernetes "no route to host"

### TLDR; flush iptable

```text
# Kubernetes 集群
systemctl stop kubelet
systemctl stop docker
iptables --flush
iptables -tnat --flush
systemctl start kubelet
systemctl start docker
```

或者你是通过 Microk8s 部署的集群，运行下方的命令

```text
# Microk8s 集群
microk8s stop
systemctl stop docker
iptables --flush
iptables -tnat --flush
systemctl start docker
microk8s start
```

### 参考

[https://github.com/kubernetes/kubeadm/issues/193\#issuecomment-330060848](https://github.com/kubernetes/kubeadm/issues/193#issuecomment-330060848)

