```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
data=[{"name":'lxh',"age":20,"cp":'lm'},{"name":'xiao',"age":40,"cp":'ly'},
{"name":'hua',"age":4,"cp":'yry'},{"name":'be',"age":70,"cp":'old'},
{"name":'hua1',"age":4,"cp":'yry'},{"name":'be2',"age":70,"cp":'old'}]
df=DataFrame(data,columns=['name','cp','age'])
print '原数据\r\n',df
```
>输出 

```
原数据
   name   cp  age
0   lxh   lm   20
1  xiao   ly   40
2   hua  yry    4
3    be  old   70
4  hua1  yry    4
5   be2  old   70
```

#head与tail#

<em>head:默认查看前5行，tail:默认查看后5行；都可以指定一个整数值M来查看从前（或后）的M行数据</em>

>示例
```python
print "使用head查看前两2行\r\n",df.head(2)
print "使用tail查看最后2行\r\n",df.tail(2)
```
>输出

```
使用head查看前两2行
   name  cp  age
0   lxh  lm   20
1  xiao  ly   40
使用tail查看最后2行
   name   cp  age
4  hua1  yry    4
5   be2  old   70
```
#算术运算与数据对齐#

<em>pandas可以对不同索引的对象进行算术运算，并自动进行数据对齐。如进行相加时，如果存在不同的索引对，则结果的索引就是该索引对的并集<em>

>算术运算包括：相加：add(),相减：sub(),相乘：mul(),相除：div()

>示例

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
#DataFrame与Series间的算术运算与数据补齐
df1 = DataFrame({'one':Series(np.array([1,-2.2,3.45]),index=['a', 'b', 'c']), 'two' : Series(np.array([2,4.2,-3.25,-5]), index=['a', 'b', 'c', 'd']),'three':Series(np.array([2,4.2,-3.25]), index=['b', 'c', 'd'])},columns=['one','two','three'])
print '原数据\r\n',df1
#取出第二行的数据
row = df1.ix[1]
print '第二行的数据\r\n',row
print '进行相加\r\n',df1.add(row,axis="columns")
#取出列"two"的数据
columns=df1["two"]
print "列two的数据\r\n",columns
print df1.add(columns,axis="index")
```
>输出

```
原数据
    one   two  three
a  1.00  2.00    NaN
b -2.20  4.20   2.00
c  3.45 -3.25   4.20
d   NaN -5.00  -3.25
第二行的数据
one     -2.2
two      4.2
three    2.0
Name: b, dtype: float64
进行相加
    one   two  three
a -1.20  6.20    NaN
b -4.40  8.40   4.00
c  1.25  0.95   6.20
d   NaN -0.80  -1.25
列two的数据
a    2.00
b    4.20
c   -3.25
d   -5.00
Name: two, dtype: float64
   one   two  three
a  3.0   4.0    NaN
b  2.0   8.4   6.20
c  0.2  -6.5   0.95
d  NaN -10.0  -8.25
```
