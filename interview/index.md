# Summary

* ### 基础知识<i id="basic" />

> POST 与 GET 请求的区别

```

```

> PHP 多继承的实现方法

```

```

* ### 中级知识<i id="middle" />

> RBAC与AUTH的区别
```
RBAC是基于角色的权限管理系统。
AUTH是基于节点的权限管理系统。
RBAC 可以将不同的用户分配给不同的用户组，即角色，每个角色享有不同的系统管理权限，但是如果角色之间的权限有交差时，可能会出现分工不明的情况，比如，有编辑与管理员两个组，编辑要求对文章列表有添加与修改的权限，管理员对文章列表有添加与修改及删除操作，还可以进行文章分类的管理，但是如果发现在编辑里有 个主编，我想让他也可以删除文章，而又不想给他文章分类管理的权限，这时候用RBAC系统除了再建立一个用户组以外，没有其它方式，至少我没想到。
而基于节点的权限管理AUTH就不会有这个问题，因为相对于所有用户来说，每个操作都是一个节点，我可以自由的分配每一个节点给每一个用户，但同时，也会出现用户组不明确的问题，就是说，没有用户组，只有用户，当然这是原本的问题，其实AUTH也是可以先建立用户组，给用户组分配权限而后再将用户分配到相应 的组里，AUTH里还有个用户组明细表，就是用来对应用户跟组的关系，这样就解决了上边所说的问题，用户没有所在组。用户又可以同时属于不同的组，进而拥 有两个组组合的权限。
```

* ### 高级部分 <i id="hight" />

> innodb下 存储函数与存储过程有什么区别

> 存储过程中用什么冰凌中止返回

> 说说node.js能解决PHP什么问题？PHP用什么交互node.js

> 说说nginx原理及传输机制

> 说说闭包原理，优点与缺点

> 一个网站分别挂在三台服务器上，通过nginx分流，有什么方案做到单点登陆

> 用过哪些搜索引擎，说说搜索引擎机制？

> mysql经常出现坏表，一般手动用命令修复，有什么方案可以防止并且做到自动修复？
```
mysqldump

```

> 对session怎么理解？session的工作原理？session工作时，为什么会开启session_start?session怎么自动存入数据库

```
1.session是一种会话控制，经常被我们用到登录控制，session经常和cookie一起用到，首先我们应该知道HTTP协议是无状态的，每个页面之间很难保持同步【登录】，这个时候就用到session了，session在使用时，如果我们不修改配置文件，每次使用都要用到session_start();这个函数开启session；然后会生成一个唯一的session_id;这个session_id;一般是储存在cookie中的，【来标注是这次会话的唯一ID】。当这次会话没结束时，再次访问的时候，就会去找这个session_id;看它有没有过期（是否存在），可以读取，就继续使用这个session_id;没有就会去重行生成一个session_id;这时如果关闭浏览器，那么这个session_id就会不存在了。

session_start();			// 开启session
$_SESSION['key'] = 'value';	// 设置session值
echo $_SESSION['key'];		// 访问session
echo $_COOKIE['PHPSESSID'];	// 访问session_id

// PHP配置文件
[Session]
session.save_handler = files 			// 默认session以文件形式保存
session.save_path = "d:/wamp/tmp"		// 设置session的保存位置
session.use_cookies = 1					// 表示会在浏览器里创建值为PHPSESSID的session_id
session.name = PHPSESSID 				// 配置session_id在cookie里面的名字
session.auto_start = 0 					// 是否默认开启session，默认不开起
session.cookie_lifetime = 0 			// 在客户端生成PHPSESSID这个cookie的过期时间，默认是0，也就是关闭浏览器就过期，下次访问，会再次生成一个session_id。所以，如果想关闭浏览器会话后，希望session信息能够保持的时间长一点，可以把这个值设置大一点，单位是秒。
session.serialize_handler = php 		// 定义用来序列化/反序列化的处理器名字。默认使用php
session.gc_divisor = 1000 				//
session.gc_probability = 1				//
session.gc_maxlifetime = 1440
// session.gc_divisor 与 session.gc_probability 合起来定义了在每个会话初始化时启动 gc（garbage collection 垃圾回收）进程的概率。此概率用 gc_probability/gc_divisor 计算得来。例如 1/100 意味着在每个请求中有 1% 的概率启动 gc 进程。session.gc_divisor 默认为 100。

// 其实也有函数来操作这些
unset($_SESSION['key']);				// 销毁某一个session变量
session_unset();						// 销毁全部session变量
session_destory();						// 销毁session文件
setcookie(session_name(),'',time()-3600,'/');//设置session_id的存取时间

// 上面说到session在配置文件里默认是以文件的形式保存【file】,它还可以保存在数据库【redis,mamached】中 
session.save_handler = redis
session.save_path = "http://127.0.0.1:6379"
```

> 对session怎么理解？session的工作原理？session工作时，为什么会开启session_start?session怎么自动存入数据库

> 对于版本控制工具git，有没有使用过？它的一些基本命令是什么？在团队合作中是否用过它？项目中线上分支和本地分支

> 是否了解面向对象？面向对象的三大特性是什么？具体介绍

> 对于mysql数据库，你所用过的存储引擎有哪些？它们的区别是什么？知道什么是锁吗？怎么理解mysql的表级锁和行级锁

> 对于PHP和Apache或者PHP和Nginx，你知道Apache和Nginx是什么吗？它们与PHP的关系是什么？它们是怎么工作的

```
1. PHP 解释器是否嵌入 Web 服务器进程内部执行

mod_php 通过嵌入 PHP 解释器到 Apache 进程中，只能与 Apache 配合使用，而 cgi 和 fast-cgi 以独立的进程的形式出现，只要对应的Web服务器实现 cgi 或者 fast-cgi 协议，就能够处理 PHP 请求。

mod_php 这种嵌入的方式最大的弊端就是内存占用大，不论是否用到 PHP 解释器都会将其加载到内存中，典型的就是处理CSS、JS之类的静态文件是完全没有必要加载解释器。

2. 单个进程处理的请求数量

mod_php 和 fast-cgi 的模式在每个进程的生命周期内能够处理多个请求(fast-cgi可以根据需要来调整进程的多少)，而 cgi 的模式处理一个请求就马上销毁进程，在高并发的场景下 cgi 的性能非常糟糕。 

每一个Web请求PHP都必须重新解析php.ini、重新载入全部dll扩展并重初始化全部数据结构。使用FastCGI，所有这些都只在进程启动时发生一次

综上，如果对性能有极高的要求，可以将静态请求和动态请求分开，这时 Nginx + php-fpm 是比较好的选择。
```