# 装饰器(Decorator)

+ 函数装饰器
+ 类装饰器

--------------------------------------------------------------------------------
## 函数装饰器
函数装饰器是包装其他函数的**函数**，也就是以被包装的函数为参数，返回包装过的函数。

用闭包实现函数装饰器，示例如下：
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

用类实现函数装饰器，也就是重载`__call__`函数：
```
class dec:
	def __init__(self, func):
		self.func = func

	def __call__(self, x):
		print("Befor func")
		self.func(x)

def foo(x):
	print("value is %d" % x)

foo = dec(foo)  # foo被赋值为dec的一个实例了
foo(1)	# 调用__call__函数, self为dec实例
```

这种通过类实现的函数装饰器，用途有一定限制，特别要注意的是装饰类的成员函数时，`self`指向了装饰器类的实例。所以普遍用闭包实现函数装饰器。

Python中使用语法糖`@`来简化装饰器的声明，多种示例如下：

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
类装饰器就是包装其他类的**函数**，也就是以被包装的**类**为参数，返回经过包装的**类**。

示例如下：
```
def dec(cls):
	class wrapper:
		def __init__(self, `*args`):
			self.wrapped = cls(`*args`)

		def __getattr__(self, name):
			return getattr(self.wrapped, name)

@dec
class foo:
	def __init__(self, x, y):
		self.attr = 'spam'

a = foo(6,7)

```
