# 异常处理

Python中，以`try/except`语句捕获异常，其格式如下：
```
try:
	statments1
except name1:
	statments2
except name2, data:
	statments3
else:
	statments4
finally:
	statments5
```

示例如下：
```
try:
	fd = open('testfile', "w")
	fd.write('This is a test')
except IOError:
	print "Here's a exception"
else:
	print "No exception"
	fd.close()
```

`except`后面带的是异常(exception)的名字，还可以带异常参数值，这个值是`raise`语句抛出异常时赋值的；`else`语句在没有发生异常时执行；
`finally`语句无论有无异常发生，都会执行，所以是作清除动作的理想位置。

在Python中，要捕获多个异常时，需要将要捕获的异常放在一个元组(Tuple)中，并作为第一个参数给`except`语句，例如：

```
try:
	l = ['a', 'b']
	int(l[2])
except (ValueError, IndexError) as e:
	pass
```
