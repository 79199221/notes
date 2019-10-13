title: Start
---

## 一、安装

```
wget http://download.redis.io/releases/redis-5.0.5.tar.gz
tar -zxvf redis-5.0.5.tar.gz
cd redis-5.0.5
make
```

## 二、启动

```
./redis-server /usr/local/redis/redis.conf
```

## 三、开机启动

```
makedir /etc/redis
cp /usr/local/redis/redis.conf /etc/redis/6379.conf
cp /usr/local/redis/utils/redis_init_script /etc/init.d/redisd
chkconfig redisd on
// 如果提示：service redisd does not support chkconfig
// 说明redisd不支持chkconfig
// 在redisd第一行加入如下注释
# chkconfig:   2345 90 10
# description:  Redis is a persistent key-value database

service redisd start
service redisd stop
```















