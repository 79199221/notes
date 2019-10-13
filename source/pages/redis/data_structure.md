title: Redis
---

---
## 〇、共同操作
```
# 判断key是否存在
> exists zhangsan
(integer) 1

# 设置过期时间（秒）
> expire zhangsan
(integer) 1

# 设置过期时间（毫秒）
> pexpire zhangsan 10000
(integer) 1

# 清空数据库
> flushdb
OK


```

## 一、String

### 添加
```
# 设置一个
> set student 'zhangsan'
OK

# 设置多个
> mset zhangsan 29 lisi 31 wangwu 33
OK

# 不存在时设置一个
> setnx zhangsan 88
(integer) 0

# 设置多个
> msetnx zhangsan 29 lisi 31 wangwu 33
(integer) 0

# 整数减一
> decr zhangsan
(integer) 28

# 整数增一
> incr zhangsan
(integer) 28

# 数字增浮点数（浮点数增浮点数会有精度问题）
> incrbyfloat number 0.5
"31.5"

# 整数增加
> incrby number 11
(integer) 33

# 整数减少
>decrby number 22
(integer) 11

# 字符串追加（不存在时设置）
> append zhangsan ' years old'
(integer) 12

# 设置过期时间（秒）
> setex mykey 10 myvalue
"myvalue"

# 设置过期时间（毫秒）
> psetex mykey 10000 myvalue
"myvalue"

# 按偏移量覆盖相同个数的字符串
> get zhangsan
"28 years old"
> setrange zhangsan 4 "sui"
(integer) 12
> get zhangsan
"28 ysuis old"

# 对字符串自定位设置值
> getbit student 0
(integer) 0
> setbit student 0 1
(integer) 0
```

### 获取
```
# 获取一个
> get student
"zhangsan"

# 获取多个
> mget student name
1) "wangwu"
2) "abcde"

# 获取子字符串
> getrange student 0 -1
"zhangsan"
> getrange student 0 3
"zhan"

# 删除
>del student
(integer) 1
```

---
## 二、Hash

---
## 三、List

---
## 四、Set

---
## 五、Zset

---
## 六、Stream

### 添加
```
> xadd student * zhangsan henan
"1569741090287-0"
> xadd student * lisi guangzhou
"1569741104701-0"
> xadd student * lisi guangdong
"1569741134395-0"
> xadd student * wangwu sichuan
"1569741158409-0"
> xadd student * lisi hebei
"1569741171652-0"
```
### 删除
```
> xdel student 1569741104701-0
(integer) 1
```
### 查看长度
```
> xlen student
(integer) 4
```
### 查看类型
```
> type student
stream
```
### 查看所有值
```
> xrange student - +
1) 1) "1569741090287-0"
   2) 1) "zhangsan"
      2) "henan"
2) 1) "1569741134395-0"
   2) 1) "lisi"
      2) "guangdong"
3) 1) "1569741158409-0"
   2) 1) "wangwu"
      2) "sichuan"
4) 1) "1569741171652-0"
   2) 1) "lisi"
      2) "hebei"
```
#### 查看 n 条值
```
> xrange student - + count 1
1) 1) "1569741090287-0"
   2) 1) "zhangsan"
      2) "henan"
```
### 查看指定范围内的值
```
> xrange student 1569741134395-0 1569741171652-0
1) 1) "1569741134395-0"
   2) 1) "lisi"
      2) "guangdong"
2) 1) "1569741158409-0"
   2) 1) "wangwu"
      2) "sichuan"
3) 1) "1569741171652-0"
   2) 1) "lisi"
      2) "hebei"
```
### 读取指定id之后的所有数据
```
> xread streams student 1569741134395-0
1) 1) "student"
   2) 1) 1) "1569741158409-0"
         2) 1) "wangwu"
            2) "sichuan"
      2) 1) "1569741171652-0"
         2) 1) "lisi"
            2) "hebei"
```
### 读取指定id之后的 n 条数据
```
> xread count 1 streams student 1569741134395-0
1) 1) "student"
   2) 1) 1) "1569741158409-0"
         2) 1) "wangwu"
            2) "sichuan"
```
# 阻塞读取数据，如果id为最新数据 阻塞n毫秒
```
> xread block 0 streams student $
> xread block 0 streams student 1569741134395-0
1) 1) "student"
   2) 1) 1) "1569741158409-0"
         2) 1) "wangwu"
            2) "sichuan"
      2) 1) "1569741171652-0"
         2) 1) "lisi"
            2) "hebei"
```


















