# 描述符(Descriptor)

在Python中，类和实例都是**名字空间**，空间中所有成员都是公开的，而且可以直接向空间中添加成员，也可以直接删除和修改空间中的成员。
为了对规范和限制对类和实例属性的操作，Python中引入了**描述符协议(Descriptor Protocol)**。

+ 描述符协议(Descriptor Protocol)
+ property类
+ 属性查找顺序

--------------------------------------------------------------------------------
## 描述符协议(Descriptor Protocol)

描述符协议(Descriptor Protocol)用来创建托管属性，其本质是将对属性的访问(读，写和删除)**绑定**到描述符协议中对应的函数(`__get__`，`__set__`，`__delete__`)，通过相应的函数来规范对属性的操作。

实现描述符协议，要满足如下条件：

1. 一个`new style`类，实现了`__get__`，`__set__`和`__delete__`函数中的任何一个
2. 以该类的**对象实例**作为另一个类的**类成员**

那么这个类`new style`类，被称为一个**描述符**。如果同时实现了`__get__`和`__set__`函数，则称之为**数据描述符**。

> 所谓`new style`类，就是基础自`object`的类，只有`new style`类可以使用`描述符协议`，`__getattribute__`等特性

上述三个函数的作用如下：
+ `__get__(self, instance, owner)`：返回属性的值，当属性不存在时，拋出`AttributeError`异常
+ `__set__(self, instance, value)`：设置属性的值，不返回任何值
+ `__delete__(self, instance)`：控制属性删除操作，不返回任何值

上述函数的参数中，当通过对象实例访问属性时，`instance`表示该对象； 通过类访问属性时，`instance`为None。`owner`表示所有者的类。

描述符协议的简单示例如下：
```
class descriptor(object):
	def __init__(self):
		pass

	def __get__(self, instance, owner):
		print "In __get__"
		return instance.__name
	
	def __set__(self, instance, value):
		print "In __set__"
		instance.__name = value
	
	def __delete__(self, instance):
		print "In __delete__"
		del instance.__name
	
class person(object):
	name = descriptor()

user = person()
user.name = 'john'
user.name
del user.name
```

--------------------------------------------------------------------------------
## 使用property类

内置的`property类`是实现数据描述符的简单方式，`property类`将对属性的访问(读，写，删除)操作映射到函数，然后可以在函数中执行参数检查。示例如下：

```
class person(object):
	def __init__(self, name):
		self._name = name	
	
	def getName(self):
		print "fetch name"
		return self._name
	
	def setName(self, name):
		print "change name"
		self._name = name
	
	def delName(self):
		print "remove name"
		del self._name
	
	name = property(getName, setName, delName, "name property docs")

bob = person('bob')
bob.name = 'john'

print bob.name
```

也可以使用把`property类`当作装饰器使用，`@property`提供属性的**读操作**，`setter`和`deleter`分别提供属性的**写操作**和**删除**，示例如下：

```
class person(object):
	@property
	def name(self): return self._name

	@name.setter
	def name(self, value): self._name = value

	@name.deleter
	def name(self): del self._name

```

使用`property类`的缺点是不能复用，即每个类属性都要重新用`property类`装饰，而`描述符(descriptor)`是可以复用的。

--------------------------------------------------------------------------------
## 属性查找顺序

在Python中，当以`obj.attr`的形式访问属性时，按如下的顺序查找属性：

1. 若`attr`是Python自动产生的属性(比如`__class__`，`__doc__`等)，则直接返回属性值
2. 在实例`obj`的类(`obj.__class__`)及其继承树中的各父类中，查看是否有名为`attr`的**数据描述符**。若有，就返回其`__get__`方法的结果；若没有，则进入下一步
3. 在`obj.__dict__`中查找`attr`。如果`obj`是一个实例，那么找到就返回，找不到就进入下一步。如果`obj`是一个类，则先要依次在其继承树中各父类的`__dict__`中，查看是否有名为`attr`的**描述符**。若有，就返回其`__get__`方法的结果；若没有，则依次在其继承树中各父类的`__dict__`中，查看是否有名为`attr`的普通属性，若有，则直接返回。若没有，则进入下一步。
4. 在`obj.__class__.__dict__`中查找，如果找到**描述符**，则返回`__get__`方法的结果；如果找到一个普通属性，则直接返回；如果没找到，则进入下一步。
5. 执行`__getattr__()`
6. 抛出`AttributeError`

**注意：**在继承树中各父类中查找时，其顺序按照[MRO算法](mro.md)执行。

+ `__getattribute__(self, name)`和`__getattr__(self, name)`的区别：

`__getattribute__`函数是访问对象实例的属性的总入口。如果重载了`__getattribute__`函数，则通过实例访问属性(即执行`obj.attr`)时，一定会调用此函数，上述属性查找顺序的`1-4`步，就是在此函数(`object.__getattribute__`)中实现的; 而`__getattr__`函数只在查找属性的最后阶段调用。

> 调用内置函数 `getattr(obj, name[, default]` 等同于`obj.name`

