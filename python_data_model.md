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

| 值类型  | 说明 |
|----------------|------|
| None           | 此类型对象只有一个值，通过内建关键字`None`访问，其布尔值为`False` |
| NotImplemented | 此类型对象只有一个值，通过内建关键字`NotImplemented`访问，其布尔值为`True`。<br/>如果类有不支持的操作，那么就可以重载这些操作对应的函数，并返回`NotImplemented` |
| Ellipsis       | 此类型对象只有一个值，通过字面量`...`或内建关键字`Ellipis`访问，其布尔值为`True` |
| number.Number  | 此类型的对象通过数字字面量来创建，或者作为算术运算的返回值。<br/>Python区分整数(integer)，浮点数(float)和复数(complex) |
| Sequences      | Sequences就是序列结构，通过整数下标或切片(slice)访问Sequences对象中的元素。<br/>Sequences分为`immutable`和`mutable`。`immutable`的有`string`，`tuple`，`bytes`；`mutable`的有`list`，`byte array` |
| Set            | Set就是集合，可分为`mutable`和`immutable`，分别通过函数`set()`和`frozenset()`创建 |
| Mapping        | Mapping就是字典 |
| Callable       | 此类型的对象可以被调用，`callable`类型的对象，有如下几种：<br/>  `User-defined function`，`Instance method`，`Generator function`，`Coroutine function`，`Asynchronous generator function`，`Built-in function`，`Built-in method`，`Classes`，`Class instance` |
| Module         | 每个Python源文件就是一个`Module`，通过`import`语句引入模块  |
| Custom Class   | 使用`class`语句定义类对象 |
| Class Instance |   |
| I/O Object     | 就是`file object`，通过`open()`，`os.popen()`，`os.fdopen()`，`makefile()`等函数创建  |
| Internal Type  | 是Python解释器使用，并暴露给用户的值，比如：<br/>  `Code Object`，`Frame Object`，`Traceback Object`，`Slice Object`，`Static Method Object`，`Class Method Object`  |

`Callable`类型是非常重要的值类型，

+ User-defined function

使用`def`语句定义函数。函数对象有一些自带的属性，如下：

| 属性名称         | 类型 | 说明 |
|------------------|------|------|
| `__doc__`          | string 或 None | 函数的Document string |
| `__name__`         | string | 函数名 |
| `__qualname__`     | string | 函数的路径 |
| `__module__`       | string | 函数所属的模块名 |
| `__defaults__`     | tuple 或 None | 存放参数的缺省值 |
| `__code__`         |  |  |
| `__globals__`      | dict | 函数中能访问的全局变量 |
| `__dict__`         | dict |  |
| `__closure__`      | tuple 或 None | 存放cell对象，每个cell对象存放闭包的一个外部变量 |
| `__annotations__`  | dict |  |
| `__kwdefaults__`   | dict | 存放命名参数的缺省值 |

+ Instance method

实例的方法也是可调用对象，其具有一些特殊的属性：

| 属性  | 说明  |
|-------|-------|
| `__self__`   | 代表实例本身 |
| `__func__`   | 代表该方法的函数体 |
| `__doc__`    | 该方法的Document string(即`__func__.__doc__`) |
| `__name__`   | 该方法的函数名(即`__func__.__name__`) |
| `__module__` | 模块名 |
 
+ Generator function

当一个函数体类使用了`yield`语句，就将此函数称为`Generator function`。此类函数被调用时，总是返回一个迭代器(Iterator Object)；调用该迭代器的`__next__()`方法，会让函数继续执行；当此类函数执行`return`或结束时，会丢出`StopIteration`异常。

+ Coroutine function

使用`async def`语句定义的函数，称为`coroutine function`。此类函数被调用时，会返回一个`coroutine object`。

+ Asynchronous generator function

使用`async def`语句定义，并且函数体内使用了`yield`语句的函数，称为`asynchronous generator function`。此类函数被调用时，会返回一个异步迭代器(`asynchronous iterator object`)

+ Built-in function

内置函数实际上是`C`语言实现的，具有很高的执行效率。

+ Built-in method


+ Classes

  类对象是可调用的，调用类对象会返回该类的一个实例(Instance)。对象有一些特殊的属性：

| 属性 | 说明 |
|-------------------|------|
| `__name__`        | 类名 |
| `__module__`      | 模块名 |
| `__dict__`        | 名字空间 |
| `__bases__`       | 基类元组 |
| `__doc__`         | Document string |
| `__annotations__` | |

  每个类对象都有一个字典对象作为其名字空间(namespace)，即`__dict__`。访问类对象的属性，都被转换为对该字典成员的访问，例如`C.x`转换为`C.__dict__["x"]`。

+ Class Instance

  定义了`__call__`方法的实例，可以当作函数调用。 类的实例有一些特殊属性：

| 属性 | 说明 |
|-------------|------|
| `__dict__`  | 存放Arbitrary Attribute的字典 |
| `__class__` | 该实例的类对象 |

  每个实例都有一个字典作为其名字空间(namespace)，即`__dict__`属性。

## 3.3 特殊函数

  Python程序中，特定的语法所代表的操作，由特殊命名的函数来实现。通过定制这些特殊函数，可以实现操作的自定义，这称为`operator overloading`。例如`x[i]`对应的操作为`type(x).__getitem__(x, i)`，因此定制类的`__getitem__`函数，就可以实现自定义的`x[i]`。
 
### 3.3.1 定制基本的类方法

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

  该函数属性访问的总入口

+ `object.__setattr__(self, name, value)`

+ `object.__delattr__(self, name)`

+ `object.__dir__(self)`

#### 3.3.2.1 实现描述符(Descriptor) 

+ `object.__get__(self, instance, owner)`
+ `object.__set__(self, instance, value)`
+ `object.__delete__(self, instance)`
+ `object.__set_name__(self, instance)`

#### 3.3.2.2 使用描述符(Descriptor)

#### 3.3.2.3 `__slots__`属性

##### 3.3.2.3.1 使用`__slots__`的注意事项

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

