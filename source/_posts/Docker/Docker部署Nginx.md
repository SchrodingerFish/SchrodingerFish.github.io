
---
title: Docker部署Nginx
date: 2026-02-03 09:45:31
categories:
  - Docker
tags:
  - Docker
  - Nginx
---

# Docker部署Nginx

## 方法一：先复制容器文件到宿主机

```bash
# 1. 先运行一个临时容器
docker run --name temp-nginx nginx:latest

# 2. 将容器内的配置文件复制到宿主机
docker cp temp-nginx:/etc/nginx ~/nginx/conf
docker cp temp-nginx:/usr/share/nginx/html ~/nginx/html
docker cp temp-nginx:/var/log/nginx ~/nginx/logs

# 3. 删除临时容器
docker rm temp-nginx

# 4. 现在可以正常运行nginx容器
docker run -d \
  --name nginx \
  -p 80:80 -p 443:443 \
  -v ~/nginx/html:/usr/share/nginx/html \
  -v ~/nginx/conf:/etc/nginx \
  -v ~/nginx/logs:/var/log/nginx \
  nginx:latest
```

## 方法二：使用初始化脚本（推荐）

创建一个初始化脚本 `init-nginx.sh`：

```bash
#!/bin/bash

NGINX_DIR=~/nginx

# 创建目录结构
mkdir -p $NGINX_DIR/{conf,html,logs}

# 检查是否已有配置文件，如果没有则从容器复制
if [ ! -f $NGINX_DIR/conf/nginx.conf ]; then
    echo "初始化nginx配置文件..."
    # 运行临时容器
    docker run --name temp-nginx nginx:latest
    # 复制配置文件
    docker cp temp-nginx:/etc/nginx/. $NGINX_DIR/conf/
    docker cp temp-nginx:/usr/share/nginx/html/. $NGINX_DIR/html/
    # 清理临时容器
    docker rm temp-nginx > /dev/null
    echo "配置文件初始化完成"
else
    echo "配置文件已存在，跳过初始化"
fi

echo "启动nginx容器..."
docker run -d \
  --name nginx \
  -p 80:80 -p 443:443 \
  -v ~/nginx/html:/usr/share/nginx/html \
  -v ~/nginx/conf:/etc/nginx \
  -v ~/nginx/logs:/var/log/nginx \
  nginx:latest

echo "Nginx容器已启动"
```

## 方法三：使用Docker Compose（最推荐）

创建 `docker-compose.yml`：

```yaml
version: '3.8'
services:
  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ~/nginx/html:/usr/share/nginx/html
      - ~/nginx/conf:/etc/nginx
      - ~/nginx/logs:/var/log/nginx
    restart: unless-stopped
```

创建初始化脚本 `setup-nginx.sh`：

```bash
#!/bin/bash

NGINX_DIR=~/nginx

mkdir -p $NGINX_DIR/{conf,html,logs}

if [ ! -f $NGINX_DIR/conf/nginx.conf ]; then
    echo "正在初始化nginx配置..."
    docker run --rm nginx:latest cat /etc/nginx/nginx.conf > $NGINX_DIR/conf/nginx.conf
    docker run --rm nginx:latest cat /etc/nginx/mime.types > $NGINX_DIR/conf/mime.types
    # 复制conf.d目录
    docker run --rm nginx:latest tar -c -C /etc/nginx/conf.d . | tar -x -C $NGINX_DIR/conf
    echo "配置初始化完成"
fi

docker-compose up -d
```

## 注意事项

1. **权限问题**：确保宿主机目录有正确的权限
2. **配置文件**：复制后记得根据需要修改 `nginx.conf`
3. **数据持久化**：这样设置后，配置和数据都会持久化在宿主机

推荐使用方法二或方法三，这样可以确保配置文件的完整性和可维护性。