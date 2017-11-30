# Python Data Model

源自Python 3.6版本的《The Python Language Reference》的第三章《Data Model》

`https://docs.python.org/3.6/reference/datamodel.html`
--------------------------------------------------------------------------------

## 3.1 对象(Object)，值(Value)和类型(Type) 
  在Python中，用对象(Object)来代表数据。每个对象(Object)都有三个属性： 标识(Identity), 类型(Type)和值(Value)。
  一旦对象被创建，其标识就不会改变，可以将标识视为对象在内存中的地址。用`id()`函数来获得对象的标识，`is`操作符就是比较两个对象的标识。
  对象的类型(Type)决定了对象所支持的操作。用`type()`函数来获得对象的类型。一旦对象被创建，其类型也是不会改变的。
  有的对象的值(Value)是可以改变的，称其为`mutable`；有的对象的值是不能改变的，称其为`immutable`。对象的值是否可以改变，由其类型(Type)决定，例如`number`, `string`和`tuple`是`immutable`，而`list`和`dictionary`是`mutable`。

> `immutable`对象的值并不是绝对的不能改变。例如当`tuple`中包含`list`时，`list`的值改变时，`tuple`值也就改变了。

  在Python中，不能显式的销毁对象，垃圾回收机制(`garbage-collected`)会回收无用的对象。

## 3.2 Python中的标准类型

下面介绍Python中内建(builtin)的类型：

+ None

  此类型对象只有一个值，通过内建关键字`None`访问，其布尔值为`False`。

+ NotImplemented

  此类型对象只有一个值，通过内建关键字`NotImplemented`访问，其布尔值为`True`。

+ Ellipsis

  此类型对象只有一个值，通过字面量`...`或内建关键字`Ellipis`访问，其布尔值为`True`。

+ number.Number

  此类型的对象通过数字字面量来创建，或者作为算术运算的返回值。Python区分整数(integer)，浮点数(float)和复数(complex)

+ Sequences

  通过整数下标或切片(slice)访问Sequences对象中的元素。Sequences分为`immutable`和`mutable`。`immutable`的有`string`，`tuple`，`bytes`；`mutable`的有`list`，`byte array`。

+ Set

  分为`set`和`frozenset`两种

+ Mapping

+ Callable
  此类型的对象可以被调用，`callable`类型的对象，有如下几种： `User-defined function`，`Instance method`，`Generator function`，`Coroutine function`，`Asynchronous generator function`，`Built-in function`，`Built-in method`，`Classes`，`Class instance`。

  - User-defined function

    使用`def`语句定义函数

  - Instance method

  - Generator function

    当一个函数体类使用了`yield`语句，就将此函数称为`Generator function`。此类函数被调用时，总是返回一个迭代器(Iterator Object)；调用该迭代器的`__next__()`方法，会让函数继续执行；当此类函数执行`return`或结束时，会丢出`StopIteration`异常。

  - Coroutine function

    使用`async def`语句定义的函数，称为`coroutine function`。此类函数被调用时，会返回一个`coroutine object`。

  - Asynchronous generator function

    使用`async def`语句定义，并且函数体内使用了`yield`语句的函数，称为`asynchronous generator function`。此类函数被调用时，会返回一个异步迭代器(`asynchronous iterator object`)

  - Built-in function

  - Built-in method

  - Classes

    类是可调用的，会返回该类的一个实例(Instance)

  - Class Instance

    定义了`__call__`方法的实例，可以当作函数调用。

+ Module

+ Custom Class

+ Class Instance

+ I/O object

+ Internal Type

  
## 3.3 特殊函数

  Python程序中，特定的语法所代表的操作，对应于特定的函数。通过定制这些特殊函数，可以实现操作的自定义，这称为`operator overloading`。例如`x[i]`对应的操作为`type(x).__getitem__(x, i)`，因此定制类的`__getitem__`函数，就可以实现自定义的`x[i]`。
 
### 3.3.1 定制基本的类方法

  关键是要搞清楚该类方法对应的场景！

+ `object.__new__(cls[,...])`
+ `object.__init__(self[,...])`
+ `object.__del__(self)`
+ `object.__repr__(self)`
+ `object.__str__(self)`
+ `object.__bytes__(self)`
+ `object.__format__(self, format_spec)`

+ `object.__lt__(self, other)`
+ `object.__le__(self, other)`
+ `object.__eq__(self, other)`
+ `object.__eq__(self, other)`
+ `object.__ne__(self, other)`
+ `object.__gt__(self, other)`
+ `object.__ge__(self, other)`

+ `object.__hash__(self)`
+ `object.__bool__(self)`
  
### 3.3.2 定制属性访问(`attribute access`)

	通过如下特殊函数，定制访问实例属性的操作(`x.name`):

+ `object.__getattr__(self, name)`

  
+ `object.__getattribute__(self, name)`
+ `object.__setattr__(self, name, value)`
+ `object.__delattr__(self, name)`
+ `object.__dir__(self)`

#### 3.3.2.1 实现描述符(Descriptor) 


+ `object.__get__(self, instance, owner)`
+ `object.__set__(self, instance, value)`
+ `object.__delete__(self, instance)`
+ `object.__set_name__(self, instance)`

#### 3.3.2.2 使用描述符(Descriptor)

#### 3.3.2.3 使用`__slots__`

### 3.3.3 定制类创建

#### 3.3.3.1 元类(`metaclass`)

### 3.3.4 定制实例和子类检查

### 3.3.5 模拟`callable`对象

### 3.3.6 模拟容器类型

### 3.3.7 模拟数字类型

### 3.3.8 上下文管理器

### 3.3.9 特殊方法的调用

## 3.4 协程(Coroutine)

### 3.4.1 `awatable`对象

### 3.4.2 `coroutine`对象

### 3.4.3 异步迭代器

### 3.4.4 异步上下文管理器

