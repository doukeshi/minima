---
layout: post
title: 使用 docker-compose 搭建 gitlab-ce
date: 2020-09-15
tags: docker， compose， gitlab
---

#### 环境要求

```
$ docker -v
$ docker-compose -v
```

#### 环境变量指定 gitlab 的安装位置
```
export GITLAB_HOME=/opt/gitlab
```

#### 创建 Compose 模板文件
```
# filename: gitlab-docker-compose.yml
version: '3'
services:
  gitlab-ce:
    image: gitlab/gitlab-ce:latest
    # restart: always
    hostname: gitlab.example.com
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://gitlab.example.com'
    ports:
      - '8080:80'
      - '8443:443'
      - '8022:22'
    volumes:
      - $GITLAB_HOME/config:/etc/gitlab
      - $GITLAB_HOME/logs:/var/log/gitlab
      - $GITLAB_HOME/data:/var/opt/gitlab
```

#### UP
```
docker-compose -f gitlab-docker-compose.yml up --remove-orphans --detach
```

#### 查看容器日志
```
$ docker logs -f bin_gitlab-ce_1
```

#### 查看容器运行状态
```
$ docker ps
CONTAINER ID  IMAGE                    COMMAND            CREATED        STATUS                  PORTS                                                              NAMES
xxxxxxxxxxxx  gitlab/gitlab-ce:latest  "/assets/wrapper"  8 minutes ago  Up 8 minutes (healthy)  0.0.0.0:8022->22/tcp, 0.0.0.0:8080->80/tcp, 0.0.0.0:8443->443/tcp  bin_gitlab-ce_1
```

#### 浏览器访问 http://127.0.0.1:8080/

----
**REF:**
> https://docs.gitlab.com/omnibus/docker/
