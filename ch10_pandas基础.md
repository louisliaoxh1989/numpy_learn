[参考文章](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dsintro)

<Strong>[Series](#Series)</Strong>

<em>&nbsp;&nbsp;[index与data的对应](#index与data的对应)</em>

<em>&nbsp;&nbsp;[创建Series](#创建Series)</em>

<em>&nbsp;&nbsp;&nbsp;&nbsp;[使用列表做为data](#使用列表做为data)</em>

<em>&nbsp;&nbsp;&nbsp;&nbsp;[使用ndarray做为data](#使用ndarray做为data)</em>

<em>&nbsp;&nbsp;&nbsp;&nbsp;[使用dict字典做为data](#使用dict字典做为data)</em>

<em>&nbsp;&nbsp;[Series值的选取](#Series值的选取)</em>

<em>&nbsp;&nbsp;&nbsp;&nbsp;[通过下标、切片、条件选取](#通过下标、切片、条件选取)</em>

<em>&nbsp;&nbsp;&nbsp;&nbsp;[通过索引值来选取](#通过索引值来选取)</em>

<Strong>[DataFrame](#DataFrame)</Strong>

<em>&nbsp;&nbsp;[创建DataFrame](#创建DataFrame)</em>

<em>&nbsp;&nbsp;&nbsp;&nbsp;[使用值为Series的字典创建](#使用值为Series的字典创建)</em>

<em>&nbsp;&nbsp;&nbsp;&nbsp;[使用字典列表(list)创建](#使用字典列表(list)创建)</em>

<em>&nbsp;&nbsp;&nbsp;&nbsp;[使用类似序列结构的字典对象创建](#使用类似序列结构的字典对象创建)</em>


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

##创建Series

<em>下面的代码是使用列表、numpy的数组、字典、数字、字符作为data来创建Series的示例</em>

###使用列表做为data###

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

>输出:

```
查看index Int64Index([0, 1, 2, 3], dtype='int64')

查看values [1 2 3 4]

查看Series

0    1

1    2

2    3

3    4

dtype: int64

查看index Index([u'a', u'b', u'c', u'd'], dtype='object')

查看values [1 2 3 4]

查看Series

a    1

b    2

c    3

d    4

dtype: int64
```

###使用ndarray做为data###

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
>输出:

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

###使用dict字典做为data###

```python
#coding=utf-8
from pandas import Series
import numpy as np

#使用字典作为ddata
dic={'CHN':100,'USA':200}
#不指定index
s=Series(dic)
print '查看index',s.index
print '查看values',s.values
print '查看Series'
print s

#指定index，且index中有原有data没有的key
index=['CHN','USA','JP']
s=Series(dic,index=index)
print '查看index',s.index
print '查看values',s.values
print '查看Series'
print s
```
>输出

```
查看index Index([u'CHN', u'USA'], dtype='object')
查看values [100 200]
查看Series
CHN    100
USA    200
dtype: int64
查看index Index([u'CHN', u'USA', u'JP'], dtype='object')
查看values [ 100.  200.   nan]
查看Series
CHN    100
USA    200
JP     NaN  #可以看到这里将data中没有的key键"JP"的值作为了NaN
dtype: float64
```

###使用单个数值作为data

```python
#coding=utf-8
from pandas import Series
import numpy as np

#使用单个数值作为data
#不指定index
s=Series(1)
print '查看index',s.index
print '查看values',s.values
print '查看Series'
print s
#指定index
s=Series(1,['a','b','c'])
print '查看index',s.index
print '查看values',s.values
print '查看Series'
print s
```
>输出

```
查看index Int64Index([0], dtype='int64')
查看values [1]
查看Series
0    1
dtype: int64
查看index Index([u'a', u'b', u'c'], dtype='object')
查看values [1 1 1]
查看Series
a    1
b    1
c    1
dtype: int64
```
##Series值的选取

##通过下标、切片、条件选取###

Numpy中用于ndarray值的选取操作很多都可用于Series的值的选择，如单个下标、切片和条件等

>示例

```python
#coding=utf-8
from pandas import Series

l=[-1, 2, 3, 10, 20, -20, 90]
s=Series(l)
#通过下标获取
print '通过下标获取第2个元素',s[1]
print '通过切片获取第1到第3个元素\r\n',s[0:3]
print '通过条件选取大于0且小于30的元素\r\n',s[(s>0)&(s<30)]
```
>输出
```
通过下标获取第2个元素 2
通过切片获取第1到第3个元素
0   -1
1    2
2    3
dtype: int64
通过条件选取大于0且小于30的元素
1     2
2     3
3    10
4    20
dtype: int64
```

###通过索引值来选取###

<em>如果在创建时指定了index则可以使用index来选取值,这个与python中的字典选取值的操作类似</em>

>示例

```python
#coding=utf-8
from pandas import Series

l=[-1, 2, 3, 10, 20, -20, 90]
#通过索引值(相当于字典中的键值)
index=['a','b','c','d','e','f','g']
s=Series(l,index=index)
print '取索引为f的元素值=',s['f']
print '取索引为e,f,g的元素值\r\n',s[['e','f','g']]
```
>输出

```
取索引为f的元素值= -20
取索引为e,f,g的元素值
e    20
f   -20
g    90
```
#DataFrame#

1. <Strong>DataFrame</Strong>是一个表格型的数据结构，它包含一组有序的列，每列可以是不同数据类型的值。可以看成是一个电子表格或一个SQL数据表或由Series组成的字典。
2. <Strong>DataFrame</Strong>既有行索引(index)，也有列索引(columns)


><Strong>语法：DataFrame(data,index,columns)</Strong>

>>
<Strong>data:可以是</Strong>

>>

```
(1) Dict of 1D ndarrays, lists, dicts, or Series
(2) 2-D numpy.ndarray
(3) Structured or record ndarray
(4)A Series
(5) Another DataFrame
```
>><Strong>index:是设置行索引，如果要指定则最好与data长度相同</Strong>

>><Strong>columns:是设置列索引，如果要指定则最好与data长度相同，如不相同则会将值设为NaN</Strong>

##创建DataFrame##

<em>下面是3种常见实用的创建方式</em>

###使用字典列表(list)创建###

>示例1：使用一组字典组成的列表作为数据值(data)创建DataFrame

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
#使用list
data=[{"name":'lxh',"age":20,"cp":'lm'},{"name":'xiao',"age":40,"cp":'ly'},
{"name":'hua',"age":4,"cp":'yry'},{"name":'be',"age":70,"cp":'old'}]
print 'data的数据类型:',type(data)
df=DataFrame(data,index=np.arange(len(data)),columns=['name','cp','age'])
print df
```
>输出

```
data的数据类型: <type 'list'>
   name   cp  age
0   lxh   lm   20
1  xiao   ly   40
2   hua  yry    4
3    be  old   70
```
><Strong>从示例中可以看出以下两点

(1). 指定了列索引colums时，得到的结果也会按指定的顺序('name','cp','age'),当指定columns时得到的值只会是columns指定的键对应的值，如果在原有data中没有该键则值设置为NaN

(2). 没有指定index时，会创建[0,1,...len(data)]的行索引

</Strong>

><Strong>下面试着将columns中的元素增加一个sex，看看会有什么效果</Strong>

```python
df=DataFrame(data,index=np.arange(len(data)),columns=['name','cp','age',"sex"])
```
>输出

```
   name   cp  age  sex
0   lxh   lm   20  NaN
1  xiao   ly   40  NaN
2   hua  yry    4  NaN
3    be  old   70  NaN
```
>><Strong>可以看出当指定的columns长度大于data或在data中没有的键时，则多出的列索引(sex)的值为NaN</Strong>

><Strong>下面试着将columns中的元素减少一个，比如age，看看会有什么效果</Strong>

```python
df=DataFrame(data,index=np.arange(len(data)),columns=['name','cp'])
```
>输出

```
   name   cp
0   lxh   lm
1  xiao   ly
2   hua  yry
3    be  old
```

>><Strong>可以看出当指定的columns长度比data键值少时，则在columns没有出现的键对应的值也不会出现</Strong>

<Strong>总结：当指定columns时得到的值只会是columns指定的键对应的值，如果在原有data中没有该键则值设置为NaN</Strong>

###使用值为Series的字典创建###

>示例2：使用一组字典其值为Series类型创建DataFrame

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
#使用带Series字典型的
data={"name":Series(["lxh", "xiao", "hua", "be"]),
       "age":Series([20, 40, 4, 70,80]),
       "cp":Series(["lm", "ly", "yry", "old"])
      }
print 'data的数据类型:',type(data)
df=DataFrame(data,columns=['name','cp','age'])
print df
```
>输出

```
data的数据类型: <type 'dict'>
   name   cp  age
0   lxh   lm   20
1  xiao   ly   40
2   hua  yry    4
3    be  old   70
4   NaN  NaN   80
```
<Strong>上面最有意思的是在设置data时的age的长度为5比name和cp的长度4不一样，也能创建成功，且将缺少的值都设置成为了NaN</Strong>

###使用类似序列结构的字典对象创建###

>示例3：使用一个能够被转换成类似序列结构的字典对象来创建一个DataFrame

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
data={"name":Series(["lxh", "xiao", "hua", "be"]),
       "age":np.arange(10,14),
       "cp":["lm", "ly", "yry", "old"]
      }
print 'data的数据类型:',type(data)
df=DataFrame(data,columns=['name','cp','age'])
print df
```
>输出

```
data的数据类型: <type 'dict'>
   name   cp  age
0   lxh   lm   10
1  xiao   ly   11
2   hua  yry   12
3    be  old   13
```

##值选取##

###通过(行)索引取某一行的数据###

><Strong>语法：df.ix[值]</Strong>

>>如果创建时没有指定index则可取值为[0,...,某个键对应值的个数-1]

>>如果指定了index则值还可以是列表中的其中一个元素值

>示例：

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
#使用list
data=[{"name":'lxh',"age":20,"cp":'lm'},{"name":'xiao',"age":40,"cp":'ly'},{"name":'hua',"age":4,"cp":'yry'},{"name":'be',"age":70,"cp":'old'}]
print 'data的数据类型:',type(data)
df=DataFrame(data,index=np.arange(len(data)),columns=['name','cp','age'])
print df
print '通过(行)索引index获取第三行的数据\r\n',df.ix[2]

df1=DataFrame(data,columns=['name','cp','age'],index=['one','two','three','four'])
print df1
print '通过(行)索引index获取第3行的数据\r\n',df1.ix[3]
print '通过索引index获取第行索引值为four的数据\r\n',df1.ix['four']
```
>输出
```
data的数据类型: <type 'list'>
   name   cp  age
0   lxh   lm   20
1  xiao   ly   40
2   hua  yry    4
3    be  old   70
通过(行)索引index获取第三行的数据
name    hua
cp      yry
age       4
Name: 2, dtype: object
       name   cp  age
one     lxh   lm   20
two    xiao   ly   40
three   hua  yry    4
four     be  old   70
通过(行)索引index获取第3行的数据
name     be
cp      old
age      70
Name: four, dtype: object
通过索引index获取第行索引值为four的数据
name     be
cp      old
age      70
Name: four, dtype: object
```
###通过列索引取某一列(键)的数据###

><Strong>语法：df[列索引值]</Strong>

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
data=[{"name":'lxh',"age":20,"cp":'lm'},{"name":'xiao',"age":40,"cp":'ly'},{"name":'hua',"age":4,"cp":'yry'},{"name":'be',"age":70,"cp":'old'}]
df=DataFrame(data,index=np.arange(len(data)),columns=['name','cp','age'])
print '通过列索引columns来获取某列如cp的数据\r\n',df["cp"]
```
>输出

```
通过列索引columns来获取某列如cp的数据
0     lm
1     ly
2    yry
3    old
Name: cp, dtype: object
```

