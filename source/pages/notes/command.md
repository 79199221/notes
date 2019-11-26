title: '常用命令'
---

```
git pull origin lht_03

// 查看逻辑CPU数
cat /proc/cpuinfo | grep "processor"|sort|uniq|wc -l
// 查看进程情况
ps -eo pid,args,psr,%mem,%cpu|grep nginx
```