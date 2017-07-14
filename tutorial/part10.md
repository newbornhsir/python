<h1 id="output">10.1.output formatting</h1>

`repr()`

```
import reprlib
reprlib.repr(set("assdfgd"))

```

`pprint`

```
>>> import pprint
>>> l = {(x,y*y) for x in range(5) for y in range(5)}
>>> l
{(3, 0), (2, 1), (0, 16), (4, 0), (4, 9), (2, 9), (1, 16), (4, 4), (0, 4), (3, 16), (4, 1), (1, 1), (2, 16), (0, 0), (1, 4), (4, 16), (3, 9), (1, 9), (1, 0), (0, 1), (3, 1), (2, 0), (0, 9), (3, 4), (2, 4)}
>>> pprint.pprint(l)
{(0, 0),
 (0, 1),
 (0, 4),
 (0, 9),
 (0, 16),
 (1, 0),
 (1, 1),
 (1, 4),
 (1, 9),
 (1, 16),
 (2, 0),
 (2, 1),
 (2, 4),
 (2, 9),
 (2, 16),
 (3, 0),
 (3, 1),
 (3, 4),
 (3, 9),
 (3, 16),
 (4, 0),
 (4, 1),
 (4, 4),
 (4, 9),
 (4, 16)}
>>> 
```
`textwrap` 以所给宽度格式化文本段落

```
>>> s = 'dhhhjajdfgwgefyugfywuvfhjdshfjfwuefgvhjadfv'
>>> import textwrap
>>> print(textwrap.fill(s,width=30))
dhhhjajdfgwgefyugfywuvfhjdshfj
fwuefgvhjadfv
>>> print(s)
dhhhjajdfgwgefyugfywuvfhjdshfjfwuefgvhjadfv
>>> 
```

`locale`模块

```
>>> import locale
>>> locale.setlocale(locale.LC_ALL, 'English_United States.1252')
'English_United States.1252'
>>> conv = locale.localeconv()          # get a mapping of conventions
>>> x = 1234567.8
>>> locale.format("%d", x, grouping=True)
'1,234,567'
>>> locale.format_string("%s%.*f", (conv['currency_symbol'],
...                      conv['frac_digits'], x), grouping=True)
'$1,234,567.80'
```


<h1 id="template">10.2.Templating</h1>

`string`模块有Template类可以让用户定制。`$`加上合法的python字符用作占位符，使用{}可以在后面跟随没有空格的数字字母字符，`%%` 是一个`%`


```
>>> from string import Template as t
>>> t1 = t('$$,$case1 ,${case2}')
>>> t1.substitute(case1='a',case2='b')
'$,a ,b'
>>> t1 = t('$$,$case1sdsd ,${case2}')  # 预想的占位符是$case1,但是后面想跟随无空格的sdsd，这种情况下需要${case1}
>>> t1.substitute(case1='a',case2='b')
Traceback (most recent call last):
  .....
KeyError: 'case1sdsd'
>>> t1 = t('$$,$case1s ,${case2}')
>>> t1.substitute(case1='a',case2='b')
Traceback (most recent call last):
  ......
KeyError: 'case1s'
>>> t1 = t('$$,$case1. ,${case2}')# 如果$case1后面无空格跟随,.之类，不报错
>>> t1.substitute(case1='a',case2='b')
'$,a. ,b'
>>> t1 = t('$$,$case1., ,${case2}')
>>> t1.substitute(case1='a',case2='b')
'$,a., ,b'
```

上述在`substitute()`方法中传入关键字参数，也可以传入字典。example:

```
>>> t1 = t('hello, $name1, i am $name2')
>>> d = dict(name1='you',name2 = 'me')
>>> t1.substitute(d)
'hello, you, i am me'
>>> t1.substitute(name1='a') #缺少name2 报错
Traceback (most recent call last):
  .....
KeyError: 'name2'
>>> t1.safe_substitute(name1='name1') #不报错
'hello, name1, i am $name2' # $name2 还是$name2
>>> 


```
然而，当使用字典或者关键字参数的时候，找不到占位符所代表的数据，或报错`KeyError`。这种情况下推荐使用`safe_substitute()`


## 未完待续





