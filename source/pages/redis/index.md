title: Redis
---

## 六、Stream

```
# 添加数据
> xadd student * zhangsan henan
"1569741090287-0"
> xadd student * lisi guangzhou
"1569741104701-0"
> xdel student 1569741104701-0
(integer) 1
> xadd student * lisi guangdong
"1569741134395-0"
> xadd student * wangwu sichuan
"1569741158409-0"
> xadd student * lisi hebei
"1569741171652-0"
#查看长度
> xlen student
(integer) 4
# 查看类型
> type student
stream
# 查看所有值
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
# 查看 n 条值
> xrange student - + count 1
1) 1) "1569741090287-0"
   2) 1) "zhangsan"
      2) "henan"
# 查看指定范围内的值
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


















