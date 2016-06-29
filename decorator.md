# 装饰器(Decorator)

+ 函数装饰器
+ 类装饰器

--------------------------------------------------------------------------------
## 函数装饰器

## 函数装饰器的写法
+ 单个Decorator，不带参数
```
@dec
def method(args):
	pass

等价于：
def method(args):
	pass
method = dec(method)
```
+ 单个Decorator, 带参数
```
@dec(params)
def method(args):
	pass

等价于：
def method(args);
	pass
method = dec(params)(method)
```

+ 多个Decorator，不带参数
```
@dec_a
@dec_b
def method(args):
	pass

等价于：
def method(args):
	pass
method = dec_a(dec_b(method))
```

+ 多个Decorator，带参数
```
@dec_a(params_b)
@dec_b(params_b)
def method(args):
	pass

等价于：
method = dec_a(params_a)(dec_b(params_b)(method))
```

--------------------------------------------------------------------------------
## 类装饰器

