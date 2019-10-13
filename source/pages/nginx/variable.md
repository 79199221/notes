title: 函数
---


> 内置变量存放在` ngx_http_core_module `模块中，
> 变量的命名方式和apache 服务器变量是一致的。
> 总而言之，这些变量代表着客户端请求头的内容，
> 例如$http_user_agent, $http_cookie, 等等。
> 下面是nginx支持的所有内置变量：

以`https://www.example.com/path/to/index.php?name=zhangsan&age=18`为例

|  函数   | 说明  | 示例
|  ----  | ----  | --- |
| $args | 请求中的参数值 |  |
| $arg_NAME | GET请求中NAME的值 |  |
| mb_​convert_​case |  |  |