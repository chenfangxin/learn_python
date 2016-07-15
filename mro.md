# MRO问题

MRO(Method Resolution Order)，即方法解析顺序，用于解决多重继承中，父类同名函数的问题。MRO实际上是要在继承树中，确定一个搜索的线性序列。

在Python中，对于经典类(Classic Class)，使用**深度优先(DFS)**原则来查找方法。示例如下：

在正常继承模式下：

```
class A:
	def m(self):
		print "In A.m"

class B:
	def m(self):
		print "In B.m"

class C(A):
	def m(self):
		print "In C.m"
		A.m(self)

class D(B):
	def m(self):
		print "In D.m"
		B.m(self)

class E(D,C):
	pass

e = E()
e.m()
```

在棱形继承模式下：
```
class A:
	def m(self):
		print "In A.m"

class B(A):
	pass

class C(A):
	def m(self):
		print "In C.m"
		A.m(self)

class D(B, C):
	pass

d = D()
d.m()
```

深度优先(DFS)算法无法解决两个问题：

+ 本地优先
本地优先的意思是说，要优先使用子类的直接父类里的方法。比如：
```
class C(B,A):
	pass
```
应该先在父类`B`的`__dict__`中查找方法，若没有则应该在父类`A`的`__dict__`中继续查找。

+ 单调性
单调性的意思是说，子类的方法查找顺序，应该与父类一致。如上例中，类`C`的查找顺序是先`B`后`A`，那么所有`C`的子类，都应该是这个顺序。

对于Python2.7中的新式类(New-style Class)以及Python3中，使用`C3算法`。


