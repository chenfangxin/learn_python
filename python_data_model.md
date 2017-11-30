# Python Data Model

源自《The Python Language Reference》的第三章《Data Model》
--------------------------------------------------------------------------------

## 对象(Object)，值(Value)和类型(Type) 
  在Python中，用对象(Object)来代表数据。每个对象(Object)都有三个属性： 标识(Identity), 类型(Type)和值(Value)。
  一旦对象被创建，其标识就不会改变，可以将标识视为对象在内存中的地址。用`id()`函数来获得对象的标识，`is`操作符就是比较两个对象的标识。
  对象的类型(Type)决定了对象所支持的操作。用`type()`函数来获得对象的类型。一旦对象被创建，其类型也是不会改变的。
  有的对象的值(Value)是可以改变的，称其为`mutable`；有的对象的值是不能改变的，称其为`immutable`。对象的值是否可以改变，由其类型(Type)决定，例如`number`, `string`和`tuple`是`immutable`，而`list`和`dictionary`是`mutable`。

> `immutable`对象的值并不是绝对的不能改变。例如当`tuple`中包含`list`时，`list`的值改变时，`tuple`值也就改变了。

  在Python中，不能显式的销毁对象，垃圾回收机制(`garbage-collected`)会回收无用的对象。

## Python中标准类型的层次

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
  `callable`类型的对象，有如下几种： `User-defined function`，`Instance method`，`Generator function`，`Coroutine function`，`Asynchronous generator function`，`Built-in function`，`Built-in method`，`Classes`，`Class instance`。

+ Module
+ Custom Class
+ Class Instance
+ I/O object
+ Internal Type

## 特殊方法

### 基本定制
### 定制属性访问
#### 实现描述符(Descriptor) 
#### 使用描述符(Descriptor)
#### 使用`__slots__`

### 定制类创建
#### 元类(`metaclass`)

### 定制实例和子类检查

### 模拟`callable`对象

### 模拟容器类型

### 模拟数子类型

### 上下文管理器

### 特殊方法的调用

## 协程(Coroutine)

### `awatable`对象

### `coroutine`对象

### 异步迭代器

### 异步上下文管理器

