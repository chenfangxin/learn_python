# MRO问题

MRO(Method Resolution Order)，即方法解析顺序，用于解决多重继承中，在多个父类中搜索函数(method)或属性(attribute)的顺序问题。MRO实际上是要在继承树中，确定一个搜索的线性序列。

在Python中，对于经典类(Classic Class)，使用**DLR算法**来查找方法。**DLR算法**就是`Deep first, from Left to Right`。

这种算法无法保证下面两个要求：

+ 本地优先，即本地属性及直接父类的属性优先

+ 单调性，即子类在继承树种搜索的顺序，应该与父类一致

对于Python2.7中的新式类(New-style Class)以及Python3中，使用`C3算法`。

## C3算法

`C3算法`实际上是要根据类的继承关系，构造一个有序表L，如下：
```
L(Child(Base1, Base2)) = [Child + merge(L(Base1), L(Base2), [Base1, Base2])]
```

L至少有一个元素（即类本身），`merge`的 规则如下：

1. 如果`merge`列表为空，则结束；若非空，取`merge`列表中第一个列表的`表头`
2. 查看该`表头`是否存在于`merge`列表的`表尾`中。若不在，进入步骤3；否则进入步骤4
3. 将`表头`放入`L`中，并从`merge`中的所有列表中删除，然后回到步骤1
4. 查看当前列表是否为merge的最后一个列表，若不是，进入步骤5；若是，进入步骤6
5. 跳过当前列表，读取`merge`中的下一个列表，然后回到步骤2
6. 丢出异常，类定义失败

`表头：` 列表中的第一个元素
`表尾：`列表中，除表头外的其他元素

如下示例：
```
class D: pass
class E: pass
class F: pass
class B(D,E): pass
class C(D,F): pass
class A(B,C): pass
```

这种继承关系，`C3`的演算过程如下:
```
L(B(D,E)) = B + merge(L(D), L(E), DE)
		  = B + merge(D, E, DE)
		  = B + D + merge(E, E)
		  = B + D + E
		  = BDE

L(C(D,F)) = C + merge(L(D), L(F), DF)
		  = C + merge(D, F, DF)
		  = C + D + merge(F, F)
		  = C + D + F
		  = CDF

L(A(B,C) = A + merge(L(B), L(C), BC)
		 = A + merge(BDE, CDF, BC)
		 = A + B + merge(DE, CDF, C)
		 = A + B + C + merge(DE, DF)
		 = A + B + C + D + merge(E, F)
		 = A + B + C + D + E + F
		 = ABCDEF
```

`super`函数就是取`L`列表中的下一个类。`super`函数被用来解决多重继承的问题。在单继承场景下，可以通过父类名直接调用父类方法，但是在多多重继承场景下，为了**一致性**，需要使用`super`函数，`super`只能用于*新式类*。

