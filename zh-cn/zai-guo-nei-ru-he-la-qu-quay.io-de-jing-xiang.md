# 在国内如何拉取 quay.io 的镜像

由于众所周知的原因，在国内无法拉取 quay.io 上的镜像。万幸的是，USTC 和 Azure 在中国境内搭建了 quay.io 的镜像。在部署 Docker 或者 Kubernetes 服务时，我们可以通过先从国内镜像拉取 image 然后重新打 tag 的方式预拉取镜像。

### 国内镜像提供商

```text
USTC: quay.mirrors.ustc.edu.cn
Azure: quay.azk8s.cn
```

以部署 Nginx Ingress 为例。查看 [https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.30.0/deploy/static/mandatory.yaml](https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.30.0/deploy/static/mandatory.yaml) 得知我们需要下载镜像

```text
quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.30.0
```

然后，我们可以通过`docker pull` 和 `docker tag` 命令拉取和 re-tag 镜像。

```bash
# docker pull <mirror address>
# docker tag <mirror address> <quay address>
docker pull quay.azk8s.cn/kubernetes-ingress-controller/nginx-ingress-controller:0.30.0
docker tag quay.azk8s.cn/kubernetes-ingress-controller/nginx-ingress-controller:0.30.0 quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.30.0
```

