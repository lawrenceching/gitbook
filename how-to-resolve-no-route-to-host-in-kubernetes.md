# How to resolve "no route to host" in Kubernetes

I ran into trouble today, without any reason, my apps in Microk8s cluster cannot resolve domain name

```text
java.lang.RuntimeException: java.net.UnknownHostException: xxx.example.com: Temporary failure in name resolution
```

And then I go to check the coredns log and see below log. `10.152.183.1:443` is the apiserver address

```text
E0528 13:48:20.680081       1 reflector.go:125] pkg/mod/k8s.io/client-go@v0.0.0-20190620085101-78d2af792bab/tools/cache/reflector.go:98: Failed to list *v1.Service: Get https://10.152.183.1:443/api/v1/services?limit=500&resourceVersion=0: dial tcp 10.152.183.1:443: connect: no route to host
```

I don't know what the root cause excactly is, but here is the solution

```text
# For Kubernetes
systemctl stop kubelet
systemctl stop docker
iptables --flush
iptables -tnat --flush
systemctl start kubelet
systemctl start docker
```

```text
# For Microk8s
microk8s stop
systemctl stop docker
iptables --flush
iptables -tnat --flush
systemctl start docker
microk8s start
```

### References

[https://github.com/kubernetes/kubeadm/issues/193\#issuecomment-330060848](https://github.com/kubernetes/kubeadm/issues/193#issuecomment-330060848)

