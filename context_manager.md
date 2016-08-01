# 场景管理器

`with ... as`语句是用来取代传统的`try ... finally`语法的。

场景管理器(`context manager`)基本思想是`with`求值的对象必须有一个`__enter__`方法和一个`__exit__`方法。
`__enter__`方法返回的对象赋值给`as`后的变量；`with ... as`后面的代码块执行完或抛出异常时，就会执行对象的`__exit__`方法。

> 定义了`__enter__`和`__exit__`方法的对象，称为场景管理器(`context manager`)。

实例如下：

```
class Test:
	def __enter__(self):
		print 'Here is __enter__'
		return 1
	def __exit__(self, *args):
		print 'Here is __exit__'

with Test as t:
	print 't is ', t
```
