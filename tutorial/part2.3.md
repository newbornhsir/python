# 2.3.Lists

python可以处理复合数据类型，其中最多的就是列表--[val1,val2]的形式。列表可以包含不同类型的数据，但通常一个列表中的数据类型是一致的

```
>>> l = [1,2,3,4]
>>> l
[1, 2, 3, 4]
>>> ll = ['a','s']
>>> ll
['a', 's']
>>> 
```

像索引和其它的内建序列一样，列表可以使用索引和分片

```
>>> l[0]
1
>>> l[1:3]
[2, 3]
>>> 

```

list是可变类型，可以更改它们的内容

```
>>> l[0]
1
>>> l[0]=2
>>> l[0]
2
```


append()为列表添加新元素

```
>>> l.append('a')
>>> l
[1, 2, 3, 4, 'a']
>>> 
```

可以使用索引为列表重新分配值

```
>>> l[1:3]=[0,0]
>>> l
[2, 0, 0, 4, 'a']
>>> l[1:3]=[]
>>> l
[2, 4, 'a']
```

创建多重列表

```
>>> z = [l,l]
>>> z
[[2, 4, 'a'], [2, 4, 'a']]

```
