# 描述符(Descriptor)

在Python中，类和实例都是**名字空间**，其所有成员都是共有的，而且可以直接添加，删除和修改名字空间中的成员。
为了对规范和限制对某些属性的操作，Python中引入了**描述符协议(Descriptor Protocol)**。

+ 描述符协议(Descriptor Protocol)
+ Property类

--------------------------------------------------------------------------------
## 描述符协议(Descriptor Protocol)
描述符协议(Descriptor Protocol)用来创建托管属性，其本质是将对属性的访问(读，写和删除)**重载**到描述符协议中对应的函数(`__get__`，`__set__`，`__delete__`)。

实现描述符协议，要满足如下条件：

1. 一个`new style`类，实现了`__get__`，`__set__`和`__delete__`函数中的任何一个
2. 以该类的**对象实例**作为另一个类的**类成员**

那么这个类`new style`类，被成为一个**描述符**。如果同时实现了`__get__`和`__set__`函数，则称之为**数据描述符**。

> 所谓`new style`类，就是基础自`object`的类

上述三个函数的作用如下：
+ `__get__(self, instance, owner)`：返回属性的值，当属性不存在时，拋出`AttributeError`异常
+ `__set__(self, instance, value)`：设置属性的值，不返回任何值
+ `__delete__(self, instance)`：控制属性删除操作，不返回任何值

描述符协议的简单示例如下：
```
class descriptor(object):
	def __init__(self):
		self.__name = ''

	def __get__(self, instance, owner):
		print "In __get__"
		return self.__name
	
	def __set__(self, instance, value):
		print "In __set__"
		self.__name = value
	
	def __delete__(self, instance):
		print "In __delete__"
		del self.__name
	
class person(object):
	name = descriptor()

user = person()
user.name = 'john'
user.name
del user.name
```



--------------------------------------------------------------------------------
## property类

内置的`property类`是实现数据描述符的简单方式，`property类`将对属性的访问(读，写，删除)操作对应到函数，然后在函数中执行检查。示例如下：
```
cass person(object):
	def __init__(self, name):
		self.__name = name	
	
	def getName(self):
		print "fetch name"
		return self.__name
	
	def setName(self, name):
		print "change name"
		self.__name = name
	
	def delName(self):
		print "remove name"
		del self.__name
	
	name = property(getName, setName, delName, "name property docs")

bob = person('bob')
bob.name = 'john'

print bob.name
```


