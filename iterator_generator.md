# 迭代器与生成器

+ 迭代(Iteration)和可迭代对象(Iterable Object)
+ 迭代器(Iterator)
+ 列表解析(List Compreha)
+ 生成器(Generator)

--------------------------------------------------------------------------------
## 迭代(Iteration)和可迭代对象(Iterable Object)
迭代就是**遍历**，是一种访问数据的方式，迭代的特点是只能向前，不能后退。迭代动作只能施加于可迭代对象(Iterable Object)上，这些对象遵守Python的**迭代协议**：
+ 可迭代对象必须具有`__iter__`函数，该函数返回一个迭代器(Iterator)对象
+ 迭代器(Iterator)必须具有`__next__`函数，每次调用该函数都返回一个数据，直到数据流的结尾丢出`StopIteration`异常

在Python中，容器是最常见的可迭代对象，而用`for循环`遍历容器，就是最常见的迭代动作。如下示例：
+ `for循环`
Python中常常用`for循环`遍历容器
```
a = [1,2,3,4,5,6]
for var in a:
	print(var)
```

在Python中，除了`for循环`，还有列表解析，`in`成员测试，`min/max`以及`map`函数都是迭代，如下示例：

+ 列表解析(List Comprehension)
列表解析是用来构造列表的，如下示例
```
[ expr for var in iterable ]
[ expr for var in iterable if cond_expr ]

```

+ `min/max`，`map`函数
```
a = [1,2,3,4,5,6]
min(a)  # 遍历iterable，查找最小值
max(a)	# 遍历iterable，查找最大值

def add(x):
	return x+1
l=list(map(add, a)) # 在Iterable的每个元素上，施加指定函数

```
--------------------------------------------------------------------------------
## 迭代器(Iterator)
迭代器(Iterator)代表了一个数据流。

--------------------------------------------------------------------------------
## 生成器(Generator)
生存器(Generator)

