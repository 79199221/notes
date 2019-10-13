title: MySQL-Router
---

## 一、Docker安装

```
FROM alpine:latest

MAINTAINER xiaozi xiaozi@ixiaozi.cn

WORKDIR ~
RUN apk update \
  && apk upgrade \
  && apk add --no-cache curl \
  && curl -o mysql-router-8.0.17-linux-glibc2.12-x86_64.tar.xz https://cdn.mysql.com//Downloads/MySQL-Router/mysql-router-8.0.17-linux-glibc2.12-x86_64.tar.xz \
  && tar -xf mysql-router-8.0.17-linux-glibc2.12-x86_64.tar.xz \
  && mv mysql-router-8.0.17-linux-glibc2.12-x86_64 /usr/local/mysql-router \
  && rm -f mysql-router-8.0.17-linux-glibc2.12-x86_64.tar.xz \
  && mkdir /etc/mysql-router \ 
  && cp /usr/local/mysql-router/share/doc/mysqlrouter/sample_mysqlrouter.conf /etc/mysql-router/mysqlrouter.conf

CMD ["sh","-c","/usr/local/mysql-router/bin/mysql-router --config=/etc/mysql-router/mysqlrouter.conf"]

```

## 二、配置
```
[DEFAULT]
logging_folder = /usr/local/mysql-router/log
plugin_folder = /usr/local/mysql-router/lib/mysqlrouter
config_folder = /usr/local/mysql-router/config
runtime_folder = /usr/local/mysql-router/run
data_folder = /usr/local/mysql-router/data

[logger]
level = INFO

[routing:basic_failover]
bind_address=127.0.0.1
bind_port = 7001
routing_strategy = first-available
mode = read-only
destinations = 127.0.0.1:3306
protocol=classic

#[routing:load_balance]
#bind_address = 127.0.0.1
#bind_port = 7002
#mode = read-only
#destinations = 127.0.0.1:3306,127.0.0.1:3306

[keepalive]
interval = 60
```