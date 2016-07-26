# 并发

Python中的并发，分为三种类型：

+ 进程
+ 线程
+ 协程

--------------------------------------------------------------------------------
+ 进程(Process)

Linux的进程基于`fork`实现。Python中，使用进程模型能绕开`GIL`，充分发挥多核性能。


--------------------------------------------------------------------------------
+ 线程(Thread)


--------------------------------------------------------------------------------
+ 协程(Coroutine)

Python中，是通过增强`generator`来实现`coroutine`的，主要有如下工作：

1. 重新定义`yield`为一个**表达式**，而不是一个**语句**
2. 给`generator`增加`send(value)`方法。通过该方法给`generator`传值，作为`yield`语句的结果
3. 给`generator`增加`throw(type[, value[, traceback]])`方法。
4. 给`generator`增加`close`方法。该方法会终止`generator`

示例如下：
```
def echo(value=None):
	try:
		while True:
			try:
				value = (yield value)
			except Exception, e:
				print 'Catch a exception'
				value = e
	finally:
		print "Don't forget to clean up when close()"

g = echo(1)
print g.next()
print g.send(2)
print g.throw(TypeError, 'spam')
print g.send(3)
g.close()
```

