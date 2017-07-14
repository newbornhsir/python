<h1 id='lists'>4.1.Lists</h1>

### 4.1.1 More on Lists

list数据类型有以下方法

* list.append(x)
    
    添加，等同于list[len(list):]=[x]

    ```
    >>> l = list()
	>>> l
	[]
	>>> l.append(1)
	>>> l
	[1]
	>>> l[len(l):]=[1]
	>>> l
	[1, 1]
    ```

* list.extend(iterable)

    通过可迭代对象扩展原有列表，等同与 list[len(list):]=[x]

    ```
    >>> l.extend(range(3))
	>>> l
	[1, 1, 0, 1, 2]
	>>> l[len(l):]=range(3,6)
	>>> l
	[1, 1, 0, 1, 2, 3, 4, 5]

    ```

* list.insert(i,x)

    在指定位置插入元素

    ```
    >>> l.insert(0,'insert')
	>>> l
	['insert', 1, 1, 0, 1, 2, 3, 4, 5]
    ```

* list.pop(i)

    移除给定位置的元素，不传入i默认移除最后一项.返回值为移除的元素

    ```
    >>> l.pop(0)
	'insert'
	>>> l
	[1, 1, 0, 1, 2, 3, 4, 5]
	>>> l.pop()
	5
	>>> l
	[1, 1, 0, 1, 2, 3, 4]
	>>> 
    ```

* list.clear()

    清楚所有元素

* list.sort(key=None,reverse=False)

    排序,两个可选参数，key指定函数比较的参数 默认值是None

* list.reverse()

    翻转列表

* list.copy()

    返回一个复制的列表

### 4.1.2 list comprehensions

列表推导提供一个简单的方法创建列表

```
>>> l = [x for x in range(6)]
>>> l
[0, 1, 2, 3, 4, 5]
>>> l = [(x,y) for x in range(0,5) for y in range(6,10)]
>>> l
[(0, 6), (0, 7), (0, 8), (0, 9), (1, 6), (1, 7), (1, 8), (1, 9), (2, 6), (2, 7), (2, 8), (2, 9), (3, 6), (3, 7), (3, 8), (3, 9), (4, 6), (4, 7), (4, 8), (4, 9)]
>>> 
```

<h1 id="del"> 4.2 del</h1>

可以通过使用索引来移除一个列表中的某一项。和pop不同，pop会返回移除的值。del还可以移除列表中的多项或者清空列表

```
>>> l = list(range(10))
>>> l
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> del l[0]
>>> l
[1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> del l[0:3]
>>> l
[4, 5, 6, 7, 8, 9]
>>> del l[:]
>>> l
[]
```
del 还可以删除整个变量

```
>>> del l
>>> l
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'l' is not defined
>>> 
```

<h1 id="tuple">4.3 元组和序列</h1>

列表和字符串有许多相同的操作，比如索引和分片。它们是序列数据类型的两个例子。元组也是一个标准序列

```
>>> t = 1,
>>> t
(1,)
>>> u = 1,2,3,t #元组是可以嵌套的
>>> u
(1, 2, 3, (1,))
>>> t[0] =2 # 元组中的数据不可以重新赋值
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> 
```

元组的输出总是包含括号，虽然在创建的时候可能没有括号，但是括号是必须的。可以创建一个包含可变对象的元组，可变对象中的数据是可变的。

虽然元组与列表很相似，但它们用于不同的情况和目的。元组是不可变类型。

创建一个空的或者包含一个元素的元组语法 t=()/t='s', t= 1,2,3 会将它们组成元组

<h1 id="sets"> 4.4. Sets</h1>

set 是python数据类型的一种-- 无序集合并且没有重复的元素。基本的用法是用于成员资格检测或者去除重复条目。

```
>>> 1,2,3,4
(1, 2, 3, 4)
>>> l = list(range(10))
>>> l
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> l.extend(range(6))
>>> l
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2, 3, 4, 5]
>>> s = set(l)
>>> s
{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
>>> 
```

set comprehension

```
>>> s = {x for x in 'assadvasdvasddsa' if x not in 'ab'}
>>> s
{'s', 'v', 'd'}
```

<h1 id="dict">字典</h1>

字典和序列不同，序列的索引是一个范围的数字，字典的索引是值，可以是任何一种不变类型。字符串或者数字经常作为key使用。如果元组只包含字符串或者数字，它可以作为keys使用。

 可以认为字典是一系列无序的键值对，并且键有着特殊的要求。一对{}创建一个空字典。

 字典的主要操作时key-value存储值，并通过key取出值。可以使用del删除一对值。如果存储值的时候使用了已经存在的key，那么以前key所对应的值会被更新.

dict()构造函数可以通过键值对序列创建字典

```
>>> d = dict([(1,2),(3,4)])
>>> d
{1: 2, 3: 4}

>>> d = dict(a=1,b=2,c=3)
>>> d
{'a': 1, 'b': 2, 'c': 3}

```

dict comprehension

```
>>> d = {x:x*2 for x in range(10)}
>>> d
{0: 0, 1: 2, 2: 4, 3: 6, 4: 8, 5: 10, 6: 12, 7: 14, 8: 16, 9: 18}

```

<h1 id="loop"> 遍历</h1>

字典可以同时遍历key-value,使用items()

```
>>> d
{'a': 1, 'b': 2, 'c': 3}
>>> for k,v in d.items():
...     print(k,v)
... 
a 1
b 2
c 3
>>> 
```

遍历序列的时候，索引和索引所代表的值可以使用enumerate()函数获得

```
>>> l = [1,2,3,4]
>>> for i,v in enumerate(l):
...     print(i,v)
... 
0 1
1 2
2 3
3 4
>>> 
```

同时遍历两个或者更多序列的时候，可以使用zip函数

```
>>> l = [1,2,3,4]
>>> l2 = 9,8,7,6
>>> for q,a in zip(l,l2):
...     print(q,a)
... 
1 9
2 8
3 7
4 6
>>> 

```

<h1 id="condition"> 4.6 more on condition</h1>

while和if的条件可以包含任何操作符。eg in /not in /is/is not/and /or

<h1 id="compare">4.7 比较序列和其它数据类型</h1>
