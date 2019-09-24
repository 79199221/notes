title: 我的文档
---

## 安装

```
# 先安装LuaJIT,并需要编译nginx_devel_kit
	./configure --prefix=/usr/local/nginx \
	--add-module=../ngx_deve_kit \
	--add-module=../lua-nginx-module \
	--with-ld-opt="-Wl,-rpth,$LUAJIT_LIB"
	
	make
	sudo make install
```

1. ngx.var.VARIABLE

2. ngx.say()
```
# 将数据作为响应体输出，返回给客户端，
# 并在末尾加上一个回车符
ngx.say('hello,world!')
```

3. lua_package_path
```
# 在配置中只能出现一次，可以是多个路径，;隔开，
# 前面找不到一次往后搜索，路径可以是
# 1.绝对路径
# 2.相对路径
# 3.${prefix} 和nginx -p 定义的路径
# ;;代表LuaJIT安装时的原始搜索路径
lua_package_path "other/path/?.lua;${prefix}conf/lua_modules/?.lua;;"
```