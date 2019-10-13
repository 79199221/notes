
title: 我的文档
---

> 安装
```
--user=www \
--group=www \
--prefix=/www/server/nginx \
--with-openssl=/www/server/nginx/src/openssl \
--add-module=/www/server/nginx/src/ngx_devel_kit \
--add-module=/www/server/nginx/src/lua_nginx_module \
--add-module=/www/server/nginx/src/ngx_cache_purge \
--add-module=/www/server/nginx/src/nginx-sticky-module \
--add-module=/www/server/nginx/src/nginx-http-concat \
--with-http_stub_status_module \
--with-http_ssl_module \
--with-http_v2_module \
--with-http_image_filter_module \
--with-http_gzip_static_module \
--with-http_gunzip_module \
--with-stream \
--with-stream_ssl_module \
--with-ipv6 \
--with-http_sub_module \
--with-http_flv_module \
--with-http_addition_module \
--with-http_realip_module \
--with-http_mp4_module \
--with-ld-opt=-Wl,-E \
--with-pcre=pcre-8.42 \
--with-cc-opt=-Wno-error \
--with-ld-opt=-ljemalloc
```

## 反射

```
	$func = new ReflectionMethod($className, $functionName);
	$func->getStartLine();	// 获取方法从类中第几行开始。
	$func->getEndLine();	// 获取方法从类中第几行结束
	
	Reflection::export(new ReflectionFunction($functionName));	// 通过方法名查找方法所在位置
	
	$func = new ReflectionClass($className);
	$func->getFileName();	//查找类名所在位置
```