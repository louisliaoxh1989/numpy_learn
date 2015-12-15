#Series

<Strong>Series</Strong><em>是一种类似于一维数组的对象，它由一组数据(values)和一组与之相关的数据标签(index索引)组成，一个简
单的Series定如下

>s=Series(data,index)

>><Strong>data</Strong>：可以是一个<em>python列表、字典、数字、字符，也可以是一个numpy的数组ndarray</em>

>><Strong>index</Strong>：是一个表示与上面data一一对应的索引值，它是一个<Strong>列表</Strong>。

##index与data的对应

<em>在创建Series对象时，index索引值的设置将会影响到创建对象值的设置</em>

(1) 使用Numpy的数组ndarray或python列表做为data，则设置的索引列表的长度必须与data的长度一致，否则会报错；也可以不设置index，会
默认生成一个[0,1,....len(data)-1]的列表做为索引

(2) 使用python字典(dict)作为data，当不指定index则会将字典的key做为索引（有序排列）；当指定了index则将指定的索引与原有
data的key做对应，如果指定的index中有原有data没有的key，则会将置为NaN

(3) 使用一个单独的字符或数值作为data，当没有指定index时则最终的值和索引都只会有一个；而当指定了index时则值的个数与index的
个数相同，且值都一样。

><strong>示例1：使用列表做为data</strong>

```python
#coding=utf-8
from pandas import Series
import numpy as np

#使用列表做为data
l=[1,2,3,4]
#不指定index
s=Series(l)
print '查看index',s.index
print '查看values',s.values
print '查看Series'
print s
#指定index
index=['a','b','c','d']
s=Series(l,index=index)
print '查看index',s.index
print '查看values',s.values
print '查看Series'
print s
```
>>输出:

>>查看index Int64Index([0, 1, 2, 3], dtype='int64')

>>查看values [1 2 3 4]

>>查看Series

>>0    1

>>1    2

>>2    3

>>3    4

>>dtype: int64

>>查看index Index([u'a', u'b', u'c', u'd'], dtype='object')

>>查看values [1 2 3 4]

>>查看Series

>>a    1

>>b    2

>>c    3

>>d    4

>>dtype: int64

><strong>示例2：使用ndarray做为data</strong>

```python
#coding=utf-8
from pandas import Series
import numpy as np

#使用列表做为data
arr=np.array([1,2,3,4])
#不指定index
s=Series(arr)
print '查看index',s.index
print '查看values',s.values
print '查看Series'
print s
#指定index
index=['a','b','c','d']
s=Series(arr,index=index)
print '查看index',s.index
print '查看values',s.values
print '查看Series'
print s
```
>>输出:

```
查看index Int64Index([0, 1, 2, 3], dtype='int64')
查看values [1 2 3 4]
查看Series
0    1
1    2
2    3
3    4
dtype: int32
查看index Index([u'a', u'b', u'c', u'd'], dtype='object')
查看values [1 2 3 4]
查看Series
a    1
b    2
c    3
d    4
dtype: int32
```
