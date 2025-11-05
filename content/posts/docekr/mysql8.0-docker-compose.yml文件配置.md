+++
date = '2025-11-05T22:45:41+08:00'
draft = false
title = 'mysql8.0中docker-compose.yml文件配置'
summary = 'Mysql 8.0 版本 docker-compose.yml配置文件记录'
categories = [ 'docker' ]
tags = [ 'docker', 'mysql' ]
+++

# docker-compose.yml文件配置
```yml
services:
  mysql:
    image: mysql:8.0
    container_name: mysql
    restart: always
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: ly15984093508  # 设置 root 密码（必需）
      TZ: Asia/Shanghai                  # 设置时区
    volumes:
      - ./data/mysql/data:/var/lib/mysql  # 数据持久化（保留）
    command:
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
```
# 启动容器服务
> docker compose up -d

# 查看容器运行状态
> docker ps

# 查看容器日志
> docker logs mysql
