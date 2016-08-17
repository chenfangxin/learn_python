# 并发

Python中的并发，分为三种类型：

+ 进程
+ 线程
+ 协程

--------------------------------------------------------------------------------
## 进程(Process)

Linux的进程基于`fork`实现。Python中，使用进程模型能绕开`GIL`，充分发挥多核性能。

+ 使用`fork`实现多进程

`fork`是系统调用，用来创建新的进程，示例如下：

```
import os
import sys

try:
	if os.fork() > 0:
		os._exit(0)
except OSError, error:
	print 'fork 1'
	os._exit(1)

os.chdir('/')
os.setsid()
os.umask(0)

try:
	pid = os.fork()
	if pid > 0:
		print 'Daemon PID %d' % pid
		os._exit(0)
except OSError, error:
	print 'fork 2'
	os._exit(1)

# redirect standard file descriptors
sys.stdout.flush()
sys.stderr.flush()
si = file('/dev/null', 'r')
so = file('/dev/null', 'a+')
se = file('/dev/null', 'a+', 0)
os.dup2(si.fileno(), sys.stdin.fileno())
os.dup2(so.fileno(), sys.stdout.fileno())
os.dup2(se.fileno(), sys.stderr.fileno())

DO_DAEMON_FUNC()

```

> 上例中，使用`os.fork()`来创建守护进程(Daemon)
> 第一次fork后，父进程退出，子进程被init接管，从而脱离终端的控制
> 第二次fork是用来禁止进程重新打开控制终端

+ 使用`multiprocessing`库


--------------------------------------------------------------------------------
## 线程(Thread)

+ 使用`threading`库

--------------------------------------------------------------------------------
## 协程(Coroutine)

### 使用`yield/send`实现`coroutine`

Python中，`yield`被用来实现`generator`。通过如下工作，利用`generator`实现`coroutine`：

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

### 使用`@asyncio.coroutine/yield from`实现`coroutine`

在Python 3.3中，引入了`yield from`语法

### 使用`async/await`实现`coroutine`

在Python 3.5中，引入了`async/await`语法

