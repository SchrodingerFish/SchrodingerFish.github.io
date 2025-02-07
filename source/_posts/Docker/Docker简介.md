---
title: Docker 简介
date: 2025-02-07 13:48:37
categories:
  - Docker
tags:
  - Docker
---

# Docker 简介

Docker是一种轻量级的容器化平台，使得应用程序及其依赖项可以在一个独立的环境中运行。本文将深入介绍Docker的常用命令，帮助你更好地使用和管理Docker容器。

{% fold info @Docker简介目录 %}
目录：

- 1. 容器生命周期管理
    - 1.1 启动容器
    - 1.2 列出运行中的容器
    - 1.3 停止容器
    - 1.4 启动已停止的容器
    - 1.5 重启容器
- 2. 容器信息查看
    - 2.1 查看容器详细信息
    - 2.2 查看容器日志
- 3. 容器交互和文件操作
    - 3.1 进入运行中的容器
    - 3.2 复制文件到容器
    - 3.3 从容器复制文件到本地
- 4. 镜像管理
    - 4.1 列出本地镜像
    - 4.2 拉取镜像
    - 4.3 删除本地镜像
    - 4.4 构建镜像
- 5. 网络管理
    - 5.1 列出网络
    - 5.2 创建网络
    - 5.3 连接容器到网络
- 6. 数据卷和数据管理
    - 6.1 创建数据卷
    - 6.2 列出数据卷
    - 6.3 挂载数据卷
    - 6.4 删除数据卷
- 7. Docker Compose
    - 7.1 创建并启动容器组
    - 7.2 停止并删除容器组
    - 7.3 查看容器组状态
- 8. 容器备份与恢复
    - 8.1 导出容器
    - 8.2 导入容器
- 9. Docker Swarm
    - 9.1 初始化Swarm
    - 9.2 创建服务
    - 9.3 列出服务
    - 9.4 扩展服务
- 10. 安全性和权限
    - 10.1 用户权限管理
    - 10.2 安全扫描
    - 10.3 Docker Bench for Security
- 11. Docker与持续集成/持续部署（CI/CD）
    - 11.1 Docker与Travis CI
    - 11.2 Docker与Jenkins
- 12. Docker与云服务
    - 12.1 Docker与AWS
    - 12.2 Docker与Azure
- 13. Docker和日志管理
    - 13.1 查看容器日志
    - 13.2 设置日志驱动
    - 13.3 指定日志标签
- 14. Docker和资源限制
    - 14.1 设置容器内存限制
    - 14.2 设置CPU共享
- 15. Docker和环境变量
    - 15.1 设置环境变量
    - 15.2 从文件加载环境变量
- 16. Docker与监控
    - 16.1 查看容器资源使用情况
    - 16.2 使用cAdvisor监控容器
- 17. Docker和自动化脚本
    - 17.1 Docker Compose自动化脚本
    - 17.2 使用Dockerfile构建自定义镜像
- 18. Docker和容器间通信
    - 18.1 容器间通信
    - 18.2 使用Link进行容器通信
- 19. 多阶段构建
- 20. Docker和服务发现
- 21. Docker和容器健康检查
- 22. Docker和GPU支持
- 23. Docker和分布式存储
- 24. Docker和自动扩展
- 总结
{% endfold %}

## **1. 容器生命周期管理**

### **1.1 启动容器**

使用`docker run`命令启动一个容器，同时指定镜像名称。

```
docker run hello-world

```

上述命令将下载并运行`hello-world`镜像。如果本地不存在该镜像，Docker将自动从Docker Hub下载。

### **1.2 列出运行中的容器**

使用`docker ps`命令列出当前正在运行的容器。

```
docker ps

```

如果需要显示所有容器（包括停止的），可以使用`-a`选项。

```
docker ps -a

```

### **1.3 停止容器**

使用`docker stop`命令停止运行中的容器。

```
docker stop <container_id>

```

### **1.4 启动已停止的容器**

使用`docker start`命令启动一个已停止的容器。

```
docker start <container_id>

```

### **1.5 重启容器**

使用`docker restart`命令重启容器。

```
docker restart <container_id>

```

## **2. 容器信息查看**

### **2.1 查看容器详细信息**

使用`docker inspect`命令查看容器的详细信息。

```
docker inspect <container_id>

```

### **2.2 查看容器日志**

使用`docker logs`命令查看容器的日志信息。

```
docker logs <container_id>

```

上述命令将显示容器的标准输出日志。如果需要实时查看日志，可以使用`-f`选项。

```
docker logs -f <container_id>

```

## **3. 容器交互和文件操作**

### **3.1 进入运行中的容器**

使用`docker exec`命令进入正在运行的容器。

```
docker exec -it <container_id> /bin/bash

```

上述命令中，`-it`选项允许交互式访问，`/bin/bash`是要执行的命令，你也可以替换成其他Shell。

### **3.2 复制文件到容器**

使用`docker cp`命令将本地文件复制到容器中。

```
docker cp /local/path/file.txt <container_id>:/container/path/file.txt

```

### **3.3 从容器复制文件到本地**

反之，可以使用`docker cp`命令将容器中的文件复制到本地。

```
docker cp <container_id>:/container/path/file.txt /local/path/file.txt

```

## **4. 镜像管理**

### **4.1 列出本地镜像**

使用`docker images`命令列出本地所有的镜像。

```
docker images

```

### **4.2 拉取镜像**

使用`docker pull`命令从Docker Hub拉取指定的镜像。

```
docker pull ubuntu:latest

```

### **4.3 删除本地镜像**

使用`docker rmi`命令删除本地的一个或多个镜像。

```
docker rmi <image_id>

```

### **4.4 构建镜像**

使用`docker build`命令根据Dockerfile构建自定义镜像。

```
docker build -t my-custom-image:latest .

```

## **5. 网络管理**

### **5.1 列出网络**

使用`docker network ls`命令列出所有网络。

```
docker network ls

```

### **5.2 创建网络**

使用`docker network create`命令创建一个新网络。

```
docker network create my-network

```

### **5.3 连接容器到网络**

使用`docker network connect`命令将容器连接到指定网络。

```
docker network connect my-network <container_id>

```

## **6. 数据卷和数据管理**

### **6.1 创建数据卷**

使用`docker volume create`命令创建一个新的数据卷。

```
docker volume create my-volume

```

### **6.2 列出数据卷**

使用`docker volume ls`命令列出所有数据卷。

```
docker volume ls

```

### **6.3 挂载数据卷**

在运行容器时，使用`-v`选项将数据卷挂载到容器内。

```
docker run -v my-volume:/container/path my-image

```

### **6.4 删除数据卷**

使用`docker volume rm`命令删除一个或多个数据卷。

```
docker volume rm my-volume

```

## **7. Docker Compose**

### **7.1 创建并启动容器组**

使用`docker-compose up`命令创建并启动Docker Compose定义的所有容器组。

```
docker-compose up

```

### **7.2 停止并删除容器组**

使用`docker-compose down`命令停止并删除Docker Compose定义的所有容器组。

```
docker-compose down

```

### **7.3 查看容器组状态**

使用`docker-compose ps`命令查看Docker Compose定义的容器组状态。

```
docker-compose ps

```

## **8. 容器备份与恢复**

### **8.1 导出容器**

使用`docker export`命令将容器导出为一个文件。

```
docker export -o my-container.tar my-container

```

### **8.2 导入容器**

使用`docker import`命令从一个文件导入容器。

```
docker import my-container.tar my-imported-container

```

## **9. Docker Swarm**

Docker Swarm是Docker官方提供的容器编排和集群管理工具。它允许将多个Docker主机组合成一个虚拟的主机，以简化容器的部署、伸缩和管理。

### **9.1 初始化Swarm**

使用`docker swarm init`命令初始化一个Swarm。

```
docker swarm init

```

此命令将输出一个加入Swarm的命令，你可以将该命令复制到其他主机上以将它们加入Swarm。

### **9.2 创建服务**

使用`docker service create`命令创建一个新的服务。

```
docker service create --name my-web-service -p 8080:80 my-web-image

```

### **9.3 列出服务**

使用`docker service ls`命令列出所有正在运行的服务。

```
docker service ls

```

### **9.4 扩展服务**

使用`docker service scale`命令扩展服务的副本数量。

```
docker service scale my-web-service=5

```

## **10. 安全性和权限**

### **10.1 用户权限管理**

Docker有一个`docker`组，只要用户属于该组，就可以在不使用`sudo`的情况下运行Docker命令。将用户添加到`docker`组：

```
sudo usermod -aG docker $USER

```

### **10.2 安全扫描**

使用Docker安全扫描工具，如Clair，对容器中的镜像进行漏洞扫描。

```
docker scan my-image

```

### **10.3 Docker Bench for Security**

Docker Bench for Security 是一个开源项目，用于检查Docker运行时的安全性设置。

```
docker run -it --net host --pid host --userns host --cap-add audit_control \
    -e DOCKER_CONTENT_TRUST=$DOCKER_CONTENT_TRUST \
    -v /var/lib:/var/lib \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /usr/lib/systemd:/usr/lib/systemd \
    -v /etc:/etc --label docker_bench_security \
    docker/docker-bench-security

```

## **11. Docker与持续集成/持续部署（CI/CD）**

### **11.1 Docker与Travis CI**

Travis CI是一个流行的持续集成服务，可以与Docker集成以构建和测试应用程序。

```
# .travis.yml
language: generic

services:
  - docker

script:
  - docker build -t my-app .
  - docker run my-app npm test

```

### **11.2 Docker与Jenkins**

Jenkins是另一个强大的持续集成工具，可以使用Docker插件构建和部署应用程序。

```
// Jenkinsfile
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    docker.build('my-app')
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image('my-app').withRun('-p 8080:80') {
                        sh 'npm test'
                    }
                }
            }
        }
    }
}

```

## **12. Docker与云服务**

### **12.1 Docker与AWS**

亚马逊AWS提供了一系列服务来支持Docker容器的运行和管理，如Amazon ECS、Amazon EKS等。

### **12.2 Docker与Azure**

微软Azure也提供了强大的Docker集成，包括Azure Container Instances和Azure Kubernetes Service等服务。

## **13. Docker和日志管理**

### **13.1 查看容器日志**

使用`docker logs`命令查看容器的标准输出日志。

```
docker logs <container_id>

```

### **13.2 设置日志驱动**

通过`--log-driver`选项，可以设置容器的日志驱动程序。

```
docker run --log-driver=syslog my-image

```

### **13.3 指定日志标签**

使用`--log-opt`选项，可以指定一些日志标签。

```
docker run --log-opt tag=my-app my-image

```

## **14. Docker和资源限制**

### **14.1 设置容器内存限制**

通过`-m`选项，可以设置容器的内存限制。

```
docker run -m 512m my-image

```

### **14.2 设置CPU共享**

通过`--cpu-shares`选项，可以设置容器的CPU共享。

```
docker run --cpu-shares 256 my-image

```

## **15. Docker和环境变量**

### **15.1 设置环境变量**

使用`-e`选项，可以在运行容器时设置环境变量。

```
docker run -e MY_VARIABLE=my-value my-image

```

### **15.2 从文件加载环境变量**

使用`--env-file`选项，可以从文件中加载环境变量。

```
docker run --env-file my-env-file my-image

```

## **16. Docker与监控**

### **16.1 查看容器资源使用情况**

使用`docker stats`命令查看运行容器的资源使用情况。

```
docker stats <container_id>

```

### **16.2 使用cAdvisor监控容器**

cAdvisor 是由Google开发的用于监控容器资源使用的工具。你可以通过以下方式使用它：

```
docker run --volume=/:/rootfs:ro --volume=/var/run:/var/run:ro --volume=/sys:/sys:ro \
    --volume=/var/lib/docker/:/var/lib/docker:ro --publish=8080:8080 \
    --detach=true --name=cadvisor google/cadvisor:latest

```

然后在浏览器中访问 http://localhost:8080 来查看监控信息。

## **17. Docker和自动化脚本**

### **17.1 Docker Compose自动化脚本**

使用`docker-compose.yml`文件来定义多容器服务，然后通过`docker-compose`命令进行管理。

```
# docker-compose.yml
version: '3'
services:
  web:
    image: nginx:alpine
  database:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example

```

```
docker-compose up -d

```

### **17.2 使用Dockerfile构建自定义镜像**

创建一个`Dockerfile`文件，定义如何构建镜像。

```
# Dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y nginx
CMD ["nginx", "-g", "daemon off;"]

```

```
docker build -t my-nginx-image .

```

## **18. Docker和容器间通信**

### **18.1 容器间通信**

使用`docker network create`命令创建一个自定义网络，然后在运行容器时指定该网络，以实现容器间的通信。

```
# 创建自定义网络
docker network create my-network

# 运行容器并加入网络
docker run --network my-network --name container-1 my-image
docker run --network my-network --name container-2 my-image

```

此时，`container-1`和`container-2`可以通过容器名相互通信。

### **18.2 使用Link进行容器通信**

通过`--link`选项，可以将一个容器连接到另一个容器，实现它们之间的通信。

```
# 运行容器并链接到其他容器
docker run --name container-1 my-image
docker run --name container-2 --link container-1:alias my-image

```

现在，`container-2`可以通过`alias`来访问`container-1`。

## **19. 多阶段构建**

多阶段构建允许你在一个`Dockerfile`中定义多个构建阶段，最终生成一个小巧的镜像。

```
# Dockerfile
# 阶段一：构建应用
FROM node:14 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# 阶段二：生成最终镜像
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html

```

## **20. Docker和服务发现**

Docker提供了内置的服务发现工具，可通过容器名或服务名来解析服务的IP地址。

```
# 使用容器名访问服务
docker run --name my-container my-image
docker exec my-container ping my-other-container

```

```
# 使用服务名访问服务
docker service create --name my-service my-image
docker exec my-container ping my-service

```

## **21. Docker和容器健康检查**

Docker容器健康检查是通过`HEALTHCHECK`指令在Dockerfile中定义的。这有助于在运行时监测容器的健康状态。

```
# Dockerfile
FROM nginx:alpine

# 添加健康检查
HEALTHCHECK --interval=5s --timeout=3s \
  CMD wget -qO- http://localhost/ || exit 1

```

运行容器时，可以使用`docker inspect`命令查看容器的健康状态。

```
docker run --name my-nginx-container my-nginx-image
docker inspect --format='{{json .State.Health.Status}}' my-nginx-container

```

## **22. Docker和GPU支持**

Docker支持在容器中使用GPU资源，这对于深度学习等GPU密集型任务非常有用。

```
# 运行容器并使用GPU
docker run --gpus all my-gpu-image

```

请注意，使用GPU需要安装NVIDIA Container Toolkit等相应的工具。

## **23. Docker和分布式存储**

Docker提供了多种方式来处理容器中的数据持久性，其中包括使用分布式存储系统。

```
# 使用Docker卷
docker run -v my-volume:/data my-image

```

此外，你还可以考虑使用分布式存储系统，如GlusterFS、Ceph等，以实现跨多个Docker主机的数据共享。

## **24. Docker和自动扩展**

Docker Swarm允许你自动扩展服务，以适应不断变化的负载。

```
# 扩展服务副本数量
docker service scale my-service=5

```

这将自动创建或销毁服务的副本，以保持指定数量的运行中容器。

## **总结**

Docker作为一种轻量级的容器化平台，通过其强大的特性和丰富的生态系统，为应用程序的构建、部署和管理提供了高效、便捷的解决方案。本文深入介绍了Docker的常用命令，覆盖了容器的生命周期管理、网络配置、资源限制、环境变量设置、多阶段构建、监控与日志、服务发现、GPU支持、分布式存储以及自动扩展等多个方面。

通过学习本文，你将掌握：

- 容器的基本生命周期管理： 启动、停止、重启、删除容器，以及查看运行中容器的状态。
- 容器间通信与网络配置： 使用自定义网络、Link选项等实现容器之间的通信，以及使用cAdvisor监控容器资源使用情况。
- 资源限制和环境变量： 设置容器内存限制、CPU共享，以及在运行容器时设置环境变量。
- Docker Compose和自动化脚本： 利用Docker Compose定义和管理多容器服务，以及使用Dockerfile构建自定义镜像。
- 多阶段构建和服务发现： 使用多阶段构建生成小巧的镜像，以及通过服务名或容器名实现容器间的服务发现。
- 容器健康检查和GPU支持： 在Dockerfile中定义容器健康检查，以及在容器中使用GPU资源。
- 分布式存储和自动扩展： 利用Docker卷处理容器中的数据持久性，以及使用Docker Swarm实现自动扩展服务。

Docker的强大功能使其成为现代应用程序开发和部署的理想选择。通过熟练掌握这些命令，你将能够更加灵活、高效地管理和运行容器化应用。
