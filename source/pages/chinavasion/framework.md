title: 我的文档
---

## 文件说明

1. 【QQN】
> 声明了数据表的静态类
```
	static public function Active() {
		return new QQNodeActive('active', null, null);
	}
```

2. 后台栏目

1. 搜索 parent_id 为null的栏目 ($parent_categories)
2. 搜索 parent_id 不为null的栏目 ($sub_categories)
---