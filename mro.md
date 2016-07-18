# MRO问题

MRO(Method Resolution Order)，即方法解析顺序，用于解决多重继承中，父类同名函数的问题。MRO实际上是要在继承树中，确定一个搜索的线性序列。

在Python中，对于经典类(Classic Class)，使用**深度优先(DFS)**原则来查找方法。**深度优先(DFS)**算法无法解决两个问题：

+ 本地优先

	本地优先的意思是说，要优先使用子类的直接父类里的方法。

+ 单调性

	单调性的意思是说，子类的方法查找顺序，应该与父类一致。


对于Python2.7中的新式类(New-style Class)以及Python3中，使用`C3算法`。

## C3算法

