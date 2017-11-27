# Python中的`coroutine`

+ 使用`yield/send`实现`coroutine`
+ 使用`@asyncio.coroutine/yield from`实现`coroutine`
+ 使用`async/await`实现`coroutine`

----------------------------------------

### 使用`yield/send`实现`coroutine`

Python中，`yield`被用来实现`generator`。`generator`与`coroutine`的共通之处在于，函数能让渡执行权，切换到其他函数执行。

通过如下工作，利用`generator`实现`coroutine`：

1. 将`yield`重新定义为一个**表达式**，而不是一个**语句**
2. 给`generator`类增加了`send(value)`方法。通过该方法给`generator`传值，作为`yield`语句的结果
3. 给`generator`类增加了`throw(type[, value[, traceback]])`方法。
4. 给`generator`类增加了`close`方法。该方法会终止`generator`

使用这些Python新增特性，构造一个简单的`Coroutine`，示例如下：

```
def grep(pattern):
	print 'Looking for ', pattern
	try:
		while True:
				line = yield
				if pattern in line:
					print line
	except GeneratorExit:
		print 'Go away, Goodbye'
		yield 'final here'

g = grep('python')
g.next() 	# 第一次对generator调用next, 会启动该generator
g.send('python generator')
a = g.throw(RuntimeError, "You're hosed, add python")

g.close()
```

生成器(`generator`)的`throw`方法有些复杂。此方法被用来向`generator`投递异常信息，以便`generator`内部能处理。

在Python 3.3中，引入了`yield from`语法，将`yield`动作委托给内层`generator`，并且可以将`send`信息传递给内层`generator`。

为了理解`yield from`，首先定义一个简单的`generator`：
```
def zero_to_nine():
	for i in range(10):
		yield i
```

如果一个函数调用了`generator`，并且还希望以`generator`的形式返回值，则必须用`for`循环重新`yield`一次，如下例：
```
def wrap_generator():
	for i in zero_to_nine():
		yield i
print list(zero_to_nine())
print list(wrap_generator())
```

现在有了`yield from`语句，就可以优雅的定义如下：
```
def process():
	yield from sub_process()

```

----------------------------------------
### 使用`@asyncio.coroutine/yield from`实现`coroutine`

`asyncio`是Python3.4版本引入的标准库。

----------------------------------------

### 使用`async/await`实现`coroutine`

在Python 3.5中，引入了关于协程的新语法——`async/await`。

