# 《fluent python》笔记

## Python数据模型
若把Python视为一个编程框架，那魔术方法(其名字以两个下划线开头，并且以两个下划线结尾)就是用户可定义并由Python调用的`HOOK`点。

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


## 函数


