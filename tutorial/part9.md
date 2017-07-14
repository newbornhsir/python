<h1 id="os">9.1.os</h1>

`os`模块提供与操作系统交互的函数


- os.getcwd()

    获取当前工作目录

- os.chdir(dir)

    改变工作目录

- os.system(command) 

    在系统shell执行语句


```
>>> os.getcwd()
'/Users/henghongwei/github/python'
>>> os.chdir('/Users/henghongwei/github/python/tutorial')
>>> os.getcwd()
'/Users/henghongwei/github/python/tutorial'
>>> os.system('npm -v')
5.2.0
0
>>> 


```

一定要使用`import os`的形式而不是`from os import *`的形式。这样会保持`os.open()`并且不会影响内置的`open()`函数的操作。

`dir()`和`help()`可以帮助了解`os`模块

完成日常文件和目录管理，`shutil`模块更加高效容易

```
>>> import shutil
>>> shutil.copyfile('data.db', 'archive.db')
'archive.db'
>>> shutil.move('/build/executables', 'installdir')
'installdir'

```

<h1 id="glob">9.2.glob</h1>

`glob`模块提供通过通配符查找文件列表的功能

```
>>> import glob
>>> glob.glob('*.md')
['part1.md', 'part2.1.md', 'part2.2.md', 'part2.3.md', 'part2.md', 'part3.md', 'part4.md', 'part5.md', 'part6.md', 'part7.md', 'part8.md', 'part9.md', 'README.md']
>>> 
```

<h1 id="argument">9.3.命令行参数</h1>


经常需要传递命令行参数。 这些参数以列表的形式存储在`sys`模块的`argv`属性中。 example:

执行 `python demo.py one two three `:

```

>>>
>>> import sys
>>> print(sys.argv)
['demo.py', 'one', 'two', 'three']

```
The `getopt` module processes sys.argv using the conventions of the Unix `getopt()` function. More powerful and flexible command line processing is provided by the `argparse` module.


<h1 id="error">9.4.Error Output Redirection and Program Termination</h1>

the sys module also has attributes for stdin, stdout, and stderr. The latter is useful for emitting warnings and error messages to make them visible even when stdout has been redirected:

```
>>>
>>> sys.stderr.write('Warning, log file not found starting a new one\n')
Warning, log file not found starting a new one

```

The most direct way to terminate a script is to use sys.exit().

<h1 id="string">9.5.字符串模式匹配</h1>

`re`模块提供正则表达式工具

```
>>> import re
>>> re.findall(r'\bf[a-z]*', 'which foot or hand fell fastest')
['foot', 'fell', 'fastest']
>>> re.sub(r'(\b[a-z]+) \1', r'\1', 'cat in the the hat')
'cat in the hat'
>>> 'tea for too'.replace('too', 'two')
'tea for two'
```

<h1 id="math">9.6.math、random、statistics</h1>

`math`模块提供数学运算，`random`随机选择，`statistics`计算数据的统计特性

<h1 id="internet">9.7.网络</h1>

`urllib.request`网络请求，`smtplib`发送邮件

```
>>> import urllib.request
>>> res = urllib.request.urlopen('http://www.baidu.com')
>>> res
<http.client.HTTPResponse object at 0x106706940>
>>> 
```


<h1 id="time">9.8.日期和时间</h1>

`datetime`模块

```
>>> import datetime
>>> date = datetime.date
>>> now = date.today()
>>> now
datetime.date(2017, 7, 14)
>>> date(1991,1,1)
datetime.date(1991, 1, 1)
>>> age = now - date(1994,1,1)
>>> age.days
8595
>>> 
```

<h1 id="data">9.9.数据压缩</h1>

以下模块都提供压缩功能：`zlib`, `gzip`, `bz2`, `lzma`, `zipfile` and `tarfile`.


<h1 id="performance">9.10.性能测试</h1>

`timetie`（测试运行时间）,`profile`,`pstats`模块

```
>>> from timeit import Timer
>>> Timer('t=a; a=b; b=t', 'a=1; b=2').timeit()
0.57535828626024577
>>> Timer('a,b = b,a', 'a=1; b=2').timeit()
0.54962537085770791
```

<h1 id="quality">9.11.质量控制</h1>

开发高品质的软件的一个方法是为每一个函数书写测试函数并在开发期间频繁测试

`doctest`模块提供一个扫面模块和验证嵌入在程序文档字符串测试的工具。测试结构和剪切粘贴一个典型的调用一样简单并将结果存入文档字符串。通过提供用户一个示例来改进文档，并且它允许doctest模块确认代码对于文档来说仍然为True:

```
>>> def test(val):
...    '''computer a list of numbers.
...    >>> print(test([1,2,3]))
...    6
...    '''
...    backval = 0
...    for n in val:
...        val += n
...    return backval
... 
>>> import doctest
>>> doctest.testmod()
**********************************************************************
File "__main__", line 3, in __main__.test
Failed example:
    print(test([1,2,3]))
Exception raised:
    Traceback (most recent call last):
      File "/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/lib/python3.6/doctest.py", line 1330, in __run
        compileflags, 1), test.globs)
      File "<doctest __main__.test[0]>", line 1, in <module>
        print(test([1,2,3]))
      File "<stdin>", line 8, in test
    TypeError: 'int' object is not iterable
**********************************************************************
1 items had failures:
   1 of   1 in __main__.test
***Test Failed*** 1 failures.
TestResults(failed=1, attempted=1)
>>> def test(val):
...     '''computer a list of numbers.
...     >>> print(test([1,2,3]))
...     6
...     '''
...     v = 0
...     for n in range(len(val)):
...         v += val[n]
...     return v
... 
>>> import doctest
>>> doctest.testmod()
TestResults(failed=0, attempted=1)
>>> 
```

`unittest`模块提供更加综合的测试，并维护在分开的文件中

```
def average(values):
    """Computes the arithmetic mean of a list of numbers.

    >>> print(average([20, 30, 70]))
    40.0
    """
    return sum(values) / len(values)

import doctest
doctest.testmod()   # automatically validate the embedded tests
```

```
import unittest

class TestStatisticalFunctions(unittest.TestCase):

    def test_average(self):
        self.assertEqual(average([20, 30, 70]), 40.0)
        self.assertEqual(round(average([1, 5, 7]), 1), 4.3)
        with self.assertRaises(ZeroDivisionError):
            average([])
        with self.assertRaises(TypeError):
            average(20, 30, 70)

unittest.main()  # Calling from the command line invokes all tests

```

<h1 id="library">9.12.自带电池</h1>

python拥有很多package,提供不同的功能





