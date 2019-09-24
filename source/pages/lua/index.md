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
> 读写Nginx的内置变量

2. ngx.say()
> 将数据作为响应体输出，返回给客户端,并在末尾加上一个回车符
```
ngx.say('hello,world!')
```

3. lua_package_path
> 定义Lua模块的搜索路径
```
# 在配置中只能出现一次，可以是多个路径，;隔开，
# 前面找不到一次往后搜索，路径可以是
# 1.绝对路径
# 2.相对路径
# 3.${prefix} 和nginx -p 定义的路径
# ;;代表LuaJIT安装时的原始搜索路径
lua_package_path "other/path/?.lua;${prefix}conf/lua_modules/?.lua;;"
```

4. lua_package_cpath
>定义C模块的搜索路径

5. ngx.req.set_header
> 添加请求头
```
# 设置单个值
ngx.req.set_header("Test_Ngx_Var","1.12.2")
# 设置多个值
ngx.req.set_header("Test",{"1","2"})
```

6. ngx.req.clear_header
> 请除请求头
```
ngx.req.clear_header("Test_Ngx_var")
ngx.req.set_header("Test_Ngx_Var",nil)
```

7. ngx.req.get_headers
> 获取请求头
```
# 返回table类型的数据
ngx.req.get_headers()
```

8. ngx_headers.HEADER
> 修改响应头