# 迭代器与生成器

+ 迭代(Iteration)和可迭代对象(Iterable Object)
+ 迭代器(Iterator)
+ 生成器(Generator)

--------------------------------------------------------------------------------
## 迭代(Iteration)和可迭代对象(Iterable Object)

迭代(`Iteration`)就是**遍历**，是一种访问数据的方式。迭代的特点是只能向前，不能后退。
迭代动作只能施加于可迭代对象(`Iterable Object`)上，可迭代对象(`Iterable Object`)遵守Python的**迭代协议(Iteration Protocol)**：

+ 可迭代对象必须具有`__iter__`函数，该函数返回一个迭代器(Iterator)对象
+ 迭代器(Iterator)必须具有`__next__`函数，每次调用该函数都返回一个数据，直到数据流的结尾丢出`StopIteration`异常

在Python中，容器是最常见的可迭代对象，用`for循环`遍历容器，就是最常见的迭代动作。如下示例：

```
a = [1,2,3,4,5,6]
for var in a:
	print(var)
```

在Python中，除了`for循环`，还有列表解析，`in`成员测试，`sum`，`min/max`以及``filter`，`map`，`reduce`函数等都用到迭代。

列表解析(List Comprehension)是用来构造列表的，如下示例
```
[ expr for var in iterable ]
[ expr for var in iterable if cond_expr ]

```

`min/max`，`map`等函数的示例如下：
```
a = [1,2,3,4,5,6]
min(a)  # 遍历iterable，查找最小值
max(a)	# 遍历iterable，查找最大值

```
#### `filter`，`map`，`reduce`的用法

`filter(function or None, iterable)` ： 以可迭代对象的每一个元素作为参数来调用布尔函数`function`，并将所有返回为`True`的元素放在一个迭代器中返回。

`map(function, *iterables)` ： 将函数`function`作用于`iterable`的每一个元素，并将结果用迭代器返回。

`reduce(function, sequence[, initial])` ： `funciton`接受两个参数，一个是`initial`，另一个是`sequence`的第一个元素。当`initial`不存在时，取`sequence`的前两个元素，将调用的结果继续作为`funciton`的一个参数，`sequence`的下一个元素作为第二个参数，继续调用`funciton`，直到`sequence`耗尽。

--------------------------------------------------------------------------------
## 迭代器(Iterator)

迭代器(`Iterator`)代表对数据流的**一次**遍历。

在Python中，迭代过程，就是先调用可迭代对象(`Iterable Object`)的`__iter__`函数，获得迭代器，
然后不断迭代器的`__next__`函数，直到抛出`StopIteration`异常。

--------------------------------------------------------------------------------
## 生成器(Generator)

有时候需要遍历的数据流非常巨大，如果要事先把这些数据都准备好，然后在其上执行迭代，就需要占用巨大的内存，效率很差。
为了解决这个问题，可以利用生成器(`Generator`),在遍历的过程中，不断推算出后续数据。

在Python中，生成器(`Generator`)是特殊的迭代器(`Iterator`)：

+ 生成器对象同时具有`__iter__`函数和`__next__`函数
+ 生成器的`__iter__`函数返回对象本身

手动打造一个生成器(Generator)，示例如下：

```
class fib:
	def __init__(self,max):
		self.max = max
		self.a = self.b = 1

	def __iter__(self):
		return self

	def __next__(self):
		if(a<max):
			cur = self.a
			self.a, self.b = self.b, self.a+self.b	
			return cur
		else:
			raise StopIteration();
	
f = fib(20)
for i in f:
	print(i)
```

Python中一般用生成器函数(`Generator Function`)来产生生成器(`Generator`)。用关键字`yield`来实现生成器函数(`Generator Function`)，`yield`是语法糖。示例如下：
```
def fib(max):
	a, b = 1, 1
	while a<max:
		yield a
		a, b = b, a+b

type(fib) # fib 是一个函数

f = fib(20)
type(f)	# f 是一个生成器(Generator)
dir(f)	# f 同时具有__iter__和__next__

for i in f:
	print(i)

```

>  生成器函数(Generator Function)与普通函数的区别是，生成器函数不用`return`返回


生成器表达式(`Generator Expressions`)是一类特殊的生成器函数，其语法与列表解析很像：
```
(expr for var in iterable)
(expr for var in iterable if cond_expr )
```

例如：
```
g = (i+1 for i in range(10) if i%2)

type(g) # g 是一个生成器
dir(g)  # g 符合生成器的条件

for n in g:
	pritn(n)
```

更多`generator`的用法示例，见[using generator](using_generator.md)。
