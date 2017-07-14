<h1 id="if">3.1.if</h1>

for example

```

>>> x = int(input('please enter an integer:'))
please enter an integer:10
>>> x
10
>>> if x > 20:
		print('x > 20')
	elif x>10:
		print('x>10 and x <=20')
	else:
		print('x<=10')
	
x<=10
```

可以有多个elif,0或者一个else,可以使用多层嵌套，即if内部再次使用if,当条件为真，执行条件后的语句，否则跳入下一次判断


<h1 id="for"> 3.2.for</h1>

for 语句和c或者pascal中相比，用法稍有不同。python中for语句涌来按照顺序迭代序列中的每一项。

```
>>> l = ['aaa','bb','cdcdc']
>>> for n in l:
		print(n,len(n))
	aaa 3
	bb 2
	cdcdc 5
```

如果你想在迭代的时候修改序列，推荐的做法是做一个含蓄的复制

```
>>> for n in l[:]:
		if len(n)>0:
			l.insert(0,n)

>>> l
['cdcdc', 'bb', 'aaa', 'aaa', 'bb', 'cdcdc']

```

<h1 id="range"> 3.3.range() 函数</h1>

如果你想迭代一个数字序列，可以使用内置的range(start,end,step)函数

```
>>> for n in range(5):
		print(n)

	
0
1
2
3
4
>>> for n in range(1,4):
		print(n)

	
1
2
3
>>> for n in range(0,10,2):
		print(n)

	
0
2
4
6
8
>>> 
```

三个参数分别是开始，结束（不包含），增量，当只传入一个参数的时候，类似（strt=0,end=val） eg: range(10) === range(0,10)。增量参数可选，默认为1


如果想通过索引迭代一个已只的序列，可以通过使用range()和len(实现):

```
>>> l
['cdcdc', 'bb', 'aaa', 'aaa', 'bb', 'cdcdc']
>>> for n in range(len(l)):
		print(n,l[n])

0 cdcdc
1 bb
2 aaa
3 aaa
4 bb
5 cdcdc

```

在许多情况下，使用enmerate()函数更加方便

```
>>> l
['cdcdc', 'bb', 'aaa', 'aaa', 'bb', 'cdcdc']
>>> s = enumerate(l)
>>> s
<enumerate object at 0x10dcc41b0>
>>> s
<enumerate object at 0x10dcc41b0>
>>> tuple(s)
((0, 'cdcdc'), (1, 'bb'), (2, 'aaa'), (3, 'aaa'), (4, 'bb'), (5, 'cdcdc'))
```

<h1 id="break">3.4.break continue else</h1>

break语句跳出for 或者while循环体

```
>>> for n in range(10):
		print(n)
		if n:
			break
0
1
>>> for n in range(0,10,2):
		for x in range(0,n):
			print('n = %d,x = %d',(n,x))
			if x == 3:
				break
n = %d,x = %d (2, 0)
n = %d,x = %d (2, 1)
n = %d,x = %d (4, 0)
n = %d,x = %d (4, 1)
n = %d,x = %d (4, 2)
n = %d,x = %d (4, 3)
n = %d,x = %d (6, 0)
n = %d,x = %d (6, 1)
n = %d,x = %d (6, 2)
n = %d,x = %d (6, 3)
n = %d,x = %d (8, 0)
n = %d,x = %d (8, 1)
n = %d,x = %d (8, 2)
n = %d,x = %d (8, 3)
```

continue只是结束本次循环进入下次循环，并不会挑出循环体：

```
>>> for n in range(10):
		if n % 2 == 0:
			continue
		else:
			print(n)
1
3
5
7
9
```
else 语句当if条件不成立的时候执行

<h1 id="pass">3.5.pass</h1>

pass 语句表示不执行任何操作。在某些情况下，只是为了让语法不出错。比如定义一个函数，但是没有开始书写函数的内容

```
def fn():
    pass

```

<h1 id="define">3.6.定义函数</h1>

```
def fn(n):
    pass

```

关键字`def`定义函数，后面必须紧随着函数名称，函数可能会传入一些参数。这个声明形成了一个函数体

函数体可以可选的声明一个文档字符串，用来介绍函数.可以通过__doc__访问

```
>>> def fn():
		"""i am function doc"""
		pass

>>> fn.__doc__
'i am function doc'
>>> 
```

内部变量和全局变量，在函数体内部分配值的变量是内部变量

```
>>> n = 1
>>> def fn():
		n = 2 #内部变量
		print(n)

	
>>> fn()
2
>>> n
1
>>> def fn2():
		print(n)

>>> fn2()
1
>>> n
1
>>> def fn3():
		n += 1 # 没有定义，报错
		print(n)

	
>>> fn3()
Traceback (most recent call last):
  File "<pyshell#160>", line 1, in <module>
    fn3()
  File "<pyshell#159>", line 2, in fn3
    n += 1
UnboundLocalError: local variable 'n' referenced before assignment

>>> def fn6():
	global n #将n变成全局变量
	n += 1
	print(n)

	
>>> fn6()
2
>>> n
2

>>> def fn4(arg):
	arg+=1
	print(arg)

	
>>> fn4(n)
2
>>> n
1
>>> def fn5(l):
		l.append(1)
		print(l)

	
>>> ls = [1,2]
>>> fn5(ls)
[1, 2, 1]
>>> ls
[1, 2, 1]
```

The execution of a function introduces a new symbol table used for the local variables of the function. More precisely, all variable assignments in a function store the value in the local symbol table; whereas variable references first look in the local symbol table, then in the local symbol tables of enclosing functions, then in the global symbol table, and finally in the table of built-in names. Thus, global variables cannot be directly assigned a value within a function (unless named in a global statement), although they may be referenced.


函数的默认返回值是None

<h1 id="function">3.7.more on define function</h1>

### 3.7.1 必须参数
这些参数在调用函数的时候必须传值

```
>>> def fn(a,b):
		print(a,b)

	
>>> fn()
Traceback (most recent call last):
  File "<pyshell#198>", line 1, in <module>
    fn()
TypeError: fn() missing 2 required positional arguments: 'a' and 'b'
>>> fn(1,2)
1 2
```
### 3.7.2 默认参数
指定一个或多个参数默认值的情况

```
>>> def f(a,b=1):
		print(a,b)

>>> f(1)
1 1
>>> f(1,2)
1 2
>>> f()
Traceback (most recent call last):
  File "<pyshell#206>", line 1, in <module>
    f()
TypeError: f() missing 1 required positional argument: 'a'
>>> 
```

具有默认值的参数在函数调用的时候如果没有传入值来更改函数参数的默认值，那么结果将使用参数的默认值。这种函数可以通过以下方式调用：

- 只传入必须要传入的参数
- 传入强制的参数和可选的参数

如果默认参数的默认值是已经定义的变量

```
>>> n = 1
>>> def f(arg = n):
		print(arg)

	
>>> f()
1
>>> n = 2
>>> f()
1
>>> l= []
>>> def f(arg = l): # 参数的默认值是可变对象
		print(arg)

	
>>> f()
[]
>>> l.append('a')
>>> f()
['a']
>>> 
```
默认值只计算一次，如果默认值是可变对象，比如列表、字典、类实例，在这种情况下会有差异。如果改变了该变量的值，函数参数的默认值也会改变。如果不想默认参数在随后的调用中共享值：

```
>>> def f(arg=None):
		if arg is None:
			print('i am none')
			arg = []
		arg.append('a')
		print(arg)
>>> f()
i am none
['a']
>>> f()
i am none
['a']
```
### 3.7.3 关键字参数

在调用函数的时候使用关键字参数调用函数(key=value的形式).eg:

```
>>> def f(a,b):
		print('a is %s,b is %s'%(a,b))
>>> f(1,2)
a is 1,b is 2
>>> f(2,1)
a is 2,b is 1
>>> f(a=1,b=2)
a is 1,b is 2
>>> f(b=2,a=1)
a is 1,b is 2
>>> f(b=2,1)
SyntaxError: positional argument follows keyword argument
>>> f(b=2,a=1)
a is 1,b is 2
>>> f(b=2,1)
SyntaxError: positional argument follows keyword argument
>>> f(a=1,2)
SyntaxError: positional argument follows keyword argument
>>> f(1,b=2)
a is 1,b is 2
>>> 
```

函数调用的时候必须关键字参数在位置参数的后面。传入的关键字参数必须在函数接收的参数范围内，顺序并不重要。函数参数只能接收一次值，多次接收会报错

```
>>> f(1,2,a=2)
Traceback (most recent call last):
  File "<pyshell#243>", line 1, in <module>
    f(1,2,a=2)
TypeError: f() got multiple values for argument 'a'
>>> 
```
### 3.7.4 任意参数 *args

```
def name(a,b=3,*args):
	pass
```
任意数量的参数组成元组。 eg:

```
>>> def f(a,b=1,*args):
		print(a,b,args)

>>> f(1,2,3,4)
1 2 (3, 4)
>>> f(1,2,range(10))
1 2 (range(0, 10),)
>>> f(1,2,*range(10))
1 2 (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
```
### 3.7.5 命名关键字参数 **kwargs

命名关键字参数组成字典的形式。eg:

```
>>> def f(*args,**kwargs):
		print(args,kwargs)
>>> f(1,2,a=1,b=2)
(1, 2) {'a': 1, 'b': 2}
>>> f(1,b=1,2,c=2)
SyntaxError: positional argument follows keyword argument
>>> l = {}
>>> l['a']=1
>>> l['b']=2
>>> l
{'a': 1, 'b': 2}
>>> f(l)
({'a': 1, 'b': 2},) {}
>>> f(**l)
() {'a': 1, 'b': 2}

```

### 3.7.6 lambda表达式

lambda关键字可以创建短的匿名函数。它的语法受限与单个表达式

```
>>> f = lambda x,y:x+y
>>> f(1,2)
3
```



参数顺序：必选参数，默认参数，可变参数，命名关键字参数，关键字参数








