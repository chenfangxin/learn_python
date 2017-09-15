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


推导(Comprehension)*只*用来生成数据结构；生成器表达式(Generator Expression)遵守迭代协议，可以逐个产生元素，节省内存。

> Python会忽略代码中(), [], {}中的换行，因此可以将较长的推导式/生成器表达式拆成多行，而不用续行符\

#### 列表(List)

* 列表推导式和生成器表达式

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

#### 元组(Tuple)

* 元组不仅仅是不可变列表，可以将元组视为记录(record)，因为元组中的元素位置是固定不变的。

## 函数


