# 《fluent python》笔记

## Python数据模型
若把Python视为一个编程框架，那魔术方法(`magic method`)(其名字以两个下划线开头，并且以两个下划线结尾)就是用户可定义并由Python调用的`HOOK`点。

在如下场景，Python会调用对应的魔术方法：
+ 迭代(Iteration)
+ 集和类(Collections)
+ 属性访问(Attribute Access)
+ 运算符重载(Operator Overloading)
+ 函数和方法调用(Function and Method Invocation)
+ 对象的创建和销毁(Object creation and destruction)
+ 字符串表示形式和格式化(String representation and formating)
+ 管理上下文(Managed contexts)

除了上述场景，Python中的内置函数也有对应的魔术方法。

## 数据结构

Python内置的数据结构有序列(sequence)，字典(dict)，集合(set)，文件(file)和字节序列()，这些都是由C语言实现的，效率非常高，而且Python的数据结构具有一致的操作接口（迭代，切片，排序，拼接等）。

序列(sequence)类型数据结构，按存放的是*值*还是*引用*来划分，可以分为`容器(Container)类型`和`扁平(Flat)类型`；若按是否能被修改来划分，可以分为`可变类型`和`不可变类型`。

|             | 容器类型                | 扁平类型                            |
|-------------|-------------------------|-------------------------------------|
| 可变类型    | list, collections.deque | bytearray, array.array, memoryview  |
| 不可变类型  | tuple                   | str, bytes                          |


推导(Comprehension)*只*用来生成数据结构，Python中的推导式可以提高代码可读性；生成器表达式(Generator Expression)遵守迭代协议，可以逐个产生元素，节省内存。

> Python会忽略代码中(), [], {}中的换行，因此可以将较长的推导式/生成器表达式拆成多行，而不用续行符\

#### 列表(List)

* 列表推导式：形式为`[value for value in INTERABLE]`

* 列表切片

  列表的切片操作seq(start:stop:step)实际上是返回一个切片对象(slice)，调用过程为：seq.__getitem__(slice(start, stop, step)
  切片除了可以读取，还可以给切片命名和赋值

* 列表拼接和增量赋值

  `seq1 + seq2`运算是将两个列表拼接，并返回一个新列表，对应的`magic method`是`__add__`

  `seq * n`运算是将列表复制`n`份，然后拼接起来，对应的`magic method`是`__mul__`

  `seq1 += seq2`运算会就地改变`seq1`，其过程取决于`seq1`，如果`seq1`实现了`__iadd__`，则调用之；如果没有实现则调用`__add__`，效果变为`seq1 = seq1 + seq2`

  `seq *= n`运算会就地改变`seq`，其对应的`magic method`是`__imul__`

* 列表排序

  list.sort方法与内置排序函数sorted的区别： list.sort是就地排序，会改变list；sorted则会返回一个新的列表，不改变原来的list。

  用`bisect`来管理已经排好序的列表

* 何时不用列表
  
  如果只需要一个只存放数字的列表，可以使用数组(`array`)


#### 元组(Tuple)

* 元组不仅仅是不可变列表，可以将元组视为记录(record)，因为元组中的元素位置是固定不变的。
* 元组拆包：体现在`平行赋值`的场景，以及函数使用元组的形式同时返回多个值的场景
* `*` 运算符，可以把一个可迭代对象拆开，用来给函数的参数赋值；在Python3中，还可以用在平行赋值的场景，用来打包不关心的元素。

#### 字典(Dict)

字典(Dict)不仅是Python中广泛使用的数据结构，也是Python语言的基石。字典的基础是`散列表`。字典中元素的`key`要求是`可散列的`。

* 字典推导：可以从任意以`键值对`为元素的`可迭代对象`中构建字典，形式为`{key:value for key, value in ITERABLE}`
* 常用字典类型：dict, defaultdict, OrderedDict

#### 集合(Set)

到Python2.6中，才把`集合`作为内置类型。`集合`中的元素必须是`可散列`的，唯一的。`集合`可以用来去重。

* 集合的分类：set, frozenset
* 集合推导：形式为`{value for value in INTERABLE}`
* 集合的操作：交集(`&`)，并集(`|`)，补集(`-`)，差集(`^`)，`in`操作，子集判断`<,<=,>,>=`等

#### 文本和字节序列

## 函数

Python中函数是`第一类值`，这意味着函数有如下特性：

1. 可以在运行时创建
2. 可以赋值给变量
3. 可以作为参数传递
4. 可以作为函数返回值

* 把函数当作对象，因此函数可以有很多`属性`，比如`__doc__`
* 高阶函数：接受函数作为参数，或者把函数当作返回值的函数
* 匿名函数：关键字`lambda`用来创建匿名函数。Python中`lambda`函数体只能用纯表达式(也就是说函数体中，不能有赋值，也不能用while，try等语句)，一般作为传给`高阶函数`的参数

#### 可调用对象
  除了函数，Python中的调用运算符`()`还可以用在其他对象上。

* 用内置函数`callable()`来判断对象能否被调用。
* Python中的7类可调用对象：用户定义的函数，内置函数，内置方法，类定义的方法，类(用类名创建实例时)，类的实例(定义了`__call__`方法)，生成器函数

> 用类名创建实例时，会先运行类的`__new__`方法创建实例，然后调用`__init__`方法初始化实例。

#### 函数的内省(Introspection)


#### 闭包与装饰器


## 面向对象

#### 引用与拷贝

#### 弱引用

