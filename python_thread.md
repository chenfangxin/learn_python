# Python多线程编程

+ 使用`threading`库

`threading`通过对`thread`模块进行二次封装，提供了方便的API来操作线程。创建线程有两种方法：
1. 继承`threading.Thread`类，并重写其`run`方法
2. 创建`threading.Thread`对象，并在其`__init__`函数中，将可调用对象传入


