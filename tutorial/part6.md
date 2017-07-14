<h1 id="format">6.1.格式化输出</h1>

目前为止，我们使用了两种方法写值：1.表达式语句2.print()函数。第三种方法是使用write()方法写入输出文件对象

格式化输出两种方式

1. 手动处理字符串，使用自己想到的操作符或者布局
2. 使用formatted string literals或者str.format()

字符串模块包含Template类，可以将值转化成字符

python有两种方法将值转换成字符。 repr() str()

str()返回代表值并且可读的字符
repr()返回解释器可识别的

如果没有可以代表并且可读的值，两种方法返回值相同

```
>>> s = 'hello'
>>> str(s)
'hello'
>>> repr(s)
"'hello'"
>>> str(fn)
'<function fn at 0x1080a2048>'
>>> repr(fn)
'<function fn at 0x1080a2048>'
>>> 

```

两种方式书写序列或者正方体表格

```
>>> for x in range(1,11):
...     print(repr(x).rjust(2),repr(x*x).rjust(3),end=' ')
...     print(repr(x*x*x).rjust(4))
... 
 1   1    1
 2   4    8
 3   9   27
 4  16   64
 5  25  125
 6  36  216
 7  49  343
 8  64  512
 9  81  729
10 100 1000
>>> for x in range(1,11):
...     print(' {0:2d} {1:3d} {2:4d}'.format(x,x*x,x*x*x))
... 
  1   1    1
  2   4    8
  3   9   27
  4  16   64
  5  25  125
  6  36  216
  7  49  343
  8  64  512
  9  81  729
 10 100 1000
>>> 

```

- str.rjust() 右对齐
- str.ljust() 左对齐
- str.center() 居中
- str.zfill() 拉长字符串，以0填充
- str.format()


```
>>> s = 'abc'
>>> s.rjust(6)
'   abc'
>>> s.ljust(6)
'abc   '
>>> s.center(6)
' abc  '
>>> s.zfill(6)
'000abc'
>>> '{0} and {1}'.format('span','div') # 数字可以用来指向通过str.format()方法传入对象的位置
'span and div'
>>> '{a} and {b}'.format(a='span',b='div') # 关键字
'span and div'
>>> '{} and {}'.format('span','div') # format的基本用法
'span and div'
>>> 
```

### old string formatting

`print('the is %s'% 'name')`


<h1 id="file">6.2 读写文件</h1>

open()返回一个文件对象，经常配合两个参数使用：open(filename,mode)

第一个参数是文件名称，第二个参数是文件打开的方式。可以是`r`只读，`w`--only write,`a`--appending,`r+`--both write and read。mode参数是可选的，默认为`r`

通常情况，文件使用文本模式打开，这意味着你可以从中读出或向其写入字符--以指定的编码方式。如果没有指定，会依赖默认平台的编码方式。`b`模式意味着以二进制模式打开文件，数据的读写以字节对象的形式，这个模式可以用于所有不包含文本的文件

在文本模式，在读文件的时候默认会转变平台指定的行尾结束符(`\n` on lunix,`\r\n` on windows)变成`\n`。当写入的时候`\n`会变成平台识别的行尾结束符。这种改变有利于文本文件但是不利于二进制数据对象如jpeg

最好的处理文件的方式是使用with关键字。这样有利于在完成操作的时候关闭文件或者在打开文件的时候出现异常

```
>>>with open('filename','w') as f:
        pass

>>>f.closed
True
```

如果你没有使用with打开文件，你应该使用close方法关闭文件。如果没有明确的关闭文件，python的垃圾回收最后也会销毁这个对象，并关闭打开的文件，但是这个文件可能会保持打开的状态一小会。另一个风险是不同的python实现清理垃圾的时刻不同

文件关闭后，再次尝试使用这个文件对象会报错


文件对象的方法

- file.read(size)

读取一定量的数据并且在text mode下返回字符串，在binary mode下返回字节对象。size是可选参数，如果忽略，会返回全部的内容。如果读到了文件的末尾，再次调用read()方法返回`''`

- file.readline()

读取一行，如果readline()返回''，说明已经读到文件末尾。可以使用循环读取整个文件

```
for line in f:
    print(line, end='')

```

- file.write()

write content and return the numbers of characters written

- file.tell()

- file.seek(offset,form_what)


使用json存储数据

```
import json

json.dumps(obj) # return json string

json.dump(x,f) # f is opened text file object, 
x = json.load(f)

```

