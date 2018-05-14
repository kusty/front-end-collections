# Docker

## 1. 基础知识

### 1.1 docker client

  > Docker client是一个泛称,用来向指定的Docker daemon发起请求,执行相应的容器管理操作.它既可以是Docker命令行工具，也可以是任何遵循了Docker API的客户端.

### 1.2 docker daemon

  > Docker daemon是Docker最核心的后台进程，它负责响应来自Docker client的请求，然后将这些请求翻译成系统调用完成容器管理操作。该进程会在后台启动一个API Server,负责接收由Docker client发送的请求；接收到的请求将通过Docker daemon内部的一个路由分发调度，再由具体的函数来执行请求。

### 1.3 docker 仓库

  > 库是集中存放镜像文件的场所。有时候会把仓库和仓库注册服务器（Registry）混为一谈，并不严格区分。实际上，仓库注册服务器上往往存放着多个仓库，每个仓库中又包含了多个镜像，每个镜像有不同的标签（tag）。仓库分为公开仓库（Public）和私有仓库（Private）两种形式。最大的公开仓库是 Docker Hub，存放了数量庞大的镜像供用户下载。 国内的公开仓库包括 Docker Pool 等，可以提供大陆用户更稳定快速的访问。当然，用户也可以在本地网络内创建一个私有仓库。当用户创建了自己的镜像之后就可以使用 push 命令将它上传到公有或者私有仓库，这样下次在另外一台机器上使用这个镜像时候，只需要从仓库上 pull 下来就可以了。
  > *注：Docker 仓库的概念跟 Git 类似，注册服务器可以理解为 GitHub 这样的托管服务。

### 1.4 docker 镜像

  > docker 镜像是一个只读的模版，用来创建docker容器。

### 1.5 docker 容器

  > docker 容器是从镜像运行的实例。可以被启动、开始、停止、删除。每个容器相互隔离。容器可以看作一个简易的linux环境和运行在其中的应用程序。


<br>
<br>

## 2. 常用命令

### 2.1 安装

  > 参照[官网](https://docs.docker.com/docker-for-mac/)

### 2.2 镜像操作

> 拉取镜像 

```
sudo docker pull ubuntu
```
> 完整写法 
```
sudo docker pull registry.hub.docker.com/ubuntu:latest
```
> 从私有仓库拉取
``` shell
sudo docker pull myregistry.com/debian:latest
```
> 列出所有本地镜像
```
docker images
```
> 创建镜像
```
 sudo docker commit -m "commit message" -a "Docker User" 0b2616b0e5a8(container_id) ouruser/sinatra:v
```
> 使用Dockerfile创建镜像
```
FROM ubunut:latest
MAINTAINER user
RUN apt-get -nginx
```
```
sudo docker build -t "tag" .
```