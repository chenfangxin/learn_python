# Python多进程编程

Linux的进程基于`fork`实现。Python中，使用进程模型能绕开`GIL`，充分发挥多核性能。

###  使用`fork`实现多进程

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

### 使用`multiprocessing`库

`multiprocessing`在Python标准库中。`multiprocessing`中以对象`Process`来表示进程。

+ 创建进程
如下代码示例创建进程：
```
from multiprocessing import Process

def f(name):
	print 'hello', name

if __name__=='__main__':
	p = Process(target=f, args=('bob',))
	p.start() # 启动进程
	p.join()  # 等候进程结束
```

+ 进程间通讯
使用`multiprocessing.Queue`和`multiprocessing.Pipes`进行进程间通讯。


+ 进程间同步
使用`multiprocessing.Lock`进行进程间同步。


+ 进程间共享数据
使用`multiprocessing.Value`和`multiprocessing.Array`进行进程间共享数据。


+ 创建进程池
使用`multiprocessing.Pool`创建进程池。
