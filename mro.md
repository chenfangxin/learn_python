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
+ 单调性

对于新式类(New-style Class)，使用`C3`算法。
