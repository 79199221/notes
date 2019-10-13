title: MySQL-Router
---

## 一、安装

```
https://dev.mysql.com/downloads/router/
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

#[routing:load_balance]
#bind_address = 127.0.0.1
#bind_port = 7002
#mode = read-only
#destinations = 127.0.0.1:3306,127.0.0.1:3306

[keepalive]
interval = 60
```