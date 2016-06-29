# 装饰器(Decorator)

+ 函数装饰器
+ 类装饰器

--------------------------------------------------------------------------------
## 函数装饰器
函数装饰器是包装其他函数的函数，也就是以被包装的函数为参数，返回包装过的函数。示例如下：
```
def dec(func):
	def wrapper(args):
		print("Before func")
		return func(args)
	return wrapper

def foo(x):
	print("value is %d" % x)

foo = dec(foo)   # foo 被装饰了, 添加了新功能

foo(1)
```

Python中使用语法糖`@`来简化装饰器的声明。

### 函数装饰器的写法
+ 单个Decorator，Decorator不带参数
```
@dec
def method(args):
	pass
```

等价于：
```
def method(args):
	pass
method = dec(method)
```

+ 单个Decorator, Decorator带参数
```
@dec(params)
def method(args):
	pass
```

等价于：
```
def method(args);
	pass
method = dec(params)(method)
```

+ 多个Decorator，Decorator不带参数
```
@dec_a
@dec_b
def method(args):
	pass
```

等价于：
```
def method(args):
	pass
method = dec_a(dec_b(method))
```

+ 多个Decorator，Decorator带参数
```
@dec_a(params_b)
@dec_b(params_b)
def method(args):
	pass

```
等价于：
```
method = dec_a(params_a)(dec_b(params_b)(method))
```

--------------------------------------------------------------------------------
## 类装饰器

