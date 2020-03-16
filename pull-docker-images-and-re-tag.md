---
description: Pull Docker images from one registry and retag them
---

# Pull Docker Images and Re-Tag

[https://gist.github.com/lawrenceching/3723e411b8580933acf0da84f6467d39](https://gist.github.com/lawrenceching/3723e411b8580933acf0da84f6467d39)

```bash
images=(
kube-apiserver:v1.17.4
kube-controller-manager:v1.17.4
kube-scheduler:v1.17.4
kube-proxy:v1.17.4
pause:3.1
etcd:3.4.3-0
coredns:1.6.5
)

for imageName in ${images[@]} ; do
docker pull gcr.azk8s.cn/google_containers/$imageName
docker tag gcr.azk8s.cn/google_containers/$imageName k8s.gcr.io/$imageName
docker rmi gcr.azk8s.cn/google_containers/$imageName
done
```

