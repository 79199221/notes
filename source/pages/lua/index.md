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

2. 