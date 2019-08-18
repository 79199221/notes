title: 我的文档
---

## 反射

```
	$func = new ReflectionMethod($className, $functionName);
	$func->getStartLine();	// 获取方法从类中第几行开始。
	$func->getEndLine();	// 获取方法从类中第几行结束
	
	Reflection::export(new ReflectionFunction($functionName));	// 通过方法名查找方法所在位置
	
	$func = new ReflectionClass($className);
	$func->getFileName();	//查找类名所在位置
```