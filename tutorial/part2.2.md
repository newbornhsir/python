# 2.2.Strings

字符串的表现方式有三种
- ''包围
- “”包围
- ‘’‘  ’‘’/ """ """ 多行字符串

```
>>> 'ss'
'ss'
>>> "ss"
'ss'
>>> '''
... dshdghsdg
... fsfd
... '''
'\ndshdghsdg\nfsfd\n'
>>> '\"sss' #转义
'"sss' # 单引号包裹
>>> '\'ss'
"'ss"
>>> 
>>> 
```

在python中，输出结果字符串以引号包围，当字符窜包含引号时候，输出使用双引号，当字符串包含双引号的时候，输出使用单引号，特殊字符需要转义输出，和原结果相同

当字符串存在\的时候,遇到特殊字符会发生转义，你可以使用原始字符串来避免这种情况

```
>>> 'sss\s'
'sss\\s' 
>>> 'sss\nss'
'sss\nss'
>>> print('sss\nss') # \n 转义成换行符
sss
ss
>>> print('sss\s')  # \s不是特使字符
sss\s
>>> print(r'sss\nss') # 使用原始字符串不发生转义
sss\nss

```


字符串的连接（+）和重复（*）

```
>>> s = "ssss"
>>> s + s
'ssssssss'
>>> s*5
'ssssssssssssssssssss'

```

两个或以上的字符串字面值靠近在一起的时候会自动连接，但是不能使用字面值和字符串变量连接(使用操作符+)

```
>>> 'ss' 'aa'
'ssaa'
>>> s 'aa'
  File "<stdin>", line 1
    s 'aa'
         ^
SyntaxError: invalid syntax
>>> 

```

字符串可以使用索引，从左至右（开始位置为0，一次加一），从右至左（开始为-1，依次减1）,超出索引报错

```
>>> s
'ssss'
>>> s[0]
's'
>>> s[-1]
's'
>>> s[10]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
>>> 
```

字符串可以使用切片，s[start:end:step],切片不会修改字符串本身，只是会返回切片的值，start为开始位置，缺省默认为0，即从字符串开始位置；end是结束位置（不包含），缺省默认取到结尾。step是步长，默认为1.

```

>>> s = 'abcdefgh'
>>> s[:]
'abcdefgh'
>>> s[0:1]
'a'
>>> s[1:]
'bcdefgh'
>>> s[::2]
'aceg'
>>> s[-3:-1]
'fg'
>>> s[-3:-5]
''
>>> s[1:121] #超出索引并不报错
'bcdefgh'
>>> 
```

python字符串是不可变类型，因此不能通过索引重新分配值，内建函数len（）可以获取到字符串的长度






