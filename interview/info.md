#info

-----
## 一. 关于项目

> 1. 关于项目
```
	项目采用lamp环境搭建，项目中url重写较多，apache认证
	项目用到memecached(windows环境没有memcached.dll，请慎重选择操作系统)
```

> 2. 关于mysql
```
	创建数据库，数据表结构关联比较复杂，为防止失败：
	导入数据前禁用外键约束：SET FOREIGN_KEY_CHECKS=0;
	导入数据后开启外键约束：SET FOREIGN_KEY_CHECKS=1;
```

> 3. 关于git
```
	git clone http://122.5.32.82:7990/scm/cv/www.git	// cv 前台
	git config --global user.name "xx"
	git config --global user.email "xx@starmerx.com"
	git config core.ignorecase false // 项目是大小写敏感的
```

> 4. 关于ubuntu
```

```