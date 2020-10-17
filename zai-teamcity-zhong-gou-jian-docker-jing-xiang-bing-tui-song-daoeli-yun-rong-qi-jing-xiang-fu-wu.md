# 在 TeamCity 中构建 Docker 镜像并推送到阿里云容器镜像服务

#### 添加阿里云私有仓库

到项目设置页面，切换到 Connections 设置页面，点击 “Add Connection“ 添加新的私有仓库

![](.gitbook/assets/image%20%2827%29.png)

![](.gitbook/assets/image%20%2824%29.png)

此处的 “Registry address“，即私有仓库地址，为阿里云容器镜像服务页面上的公网地址。

![](.gitbook/assets/image%20%2823%29.png)

#### 添加 Docker 构建步骤

TeamCity 内置了 Docker 构建步骤。新建 Docker build 和 Docker push 命令。 其中的 "Image name: tag" 为你的私有项目地址。这里我的 tag 为 "1.0.%build.counter%"。"%build.counter%" 为 TeamCity 自己的系统变量。每次构建，build.counter 会自增一位。通过这种方式，每次构建都会打上一个新的 tag 作为版本号。

![](.gitbook/assets/image%20%2822%29.png)

![](.gitbook/assets/image%20%2826%29.png)

你还可以打上不同的 tag，例如这样

![](.gitbook/assets/image%20%2828%29.png)



