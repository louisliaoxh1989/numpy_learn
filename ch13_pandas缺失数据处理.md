
#查看数据中那些有缺失值NaN#

<Strong>对于Series和DataFrame都可以使用方法isNull来查看数据中缺失值的布尔值，并结合使用any或all方法来判定某一列或某一行是否有缺失值<Strong>

```python
#coding=utf-8
from pandas import Series,DataFrame
df = DataFrame({'one':Series(np.array([1,-2.2,1]),index=['a', 'b', 'c']), 'two' : Series(np.array([2,4.2,-3.25,-5]), index=['a', 'b', 'c', 'd']),'three':Series(np.array([2,2,4.2,-3.25]), index=['a','b', 'c', 'd'])},columns=['one','two','three'])
print '原值\r\n',df
df = DataFrame({'one':Series(np.array([1,-2.2,1]),index=['a', 'b', 'c']), 'two' : Series(np.array([2,4.2,-3.25,-5]), index=['a', 'b', 'c', 'd']),'three':Series(np.array([2,2,4.2,-3.25]), index=['a','b', 'c', 'd'])},columns=['one','two','three'])

print '原值\r\n',df
print '查看缺失值的情况\r\n',df.isnull() #也可以用notnull来判断那些值不是缺失值
print '查看列one缺失值的情况\r\n',df["one"].isnull()
print '判断列one是否有缺失值：',df["one"].isnull().any()
```
```
原值
   one   two  three
a  1.0  2.00   2.00
b -2.2  4.20   2.00
c  1.0 -3.25   4.20
d  NaN -5.00  -3.25
查看缺失值的情况
     one    two  three
a  False  False  False
b  False  False  False
c  False  False  False
d   True  False  False
查看列one缺失值的情况
a    False
b    False
c    False
d     True
Name: one, dtype: bool
判断列one是否有缺失值： True
```

#填充缺失值#

<Strong>对于Series和DataFrame都可以使用方法fillna()来将缺失值进行替换</Strong>

<em>该方法的参数说明如下</em>

```
(1) value ：用于填充缺失值的值或字典对象
(2) methon：插值方式。如果在调用时未指定其他参数时，默认为"ffill"
(3) axis：  待填充的轴，默认axis=0
(3) inplace：修改调用者而不产生副本，默认为False，即调用fillna()会产生新对象，不会对原有对象产生影响
(4) limit：可以连续填充的最大数量
```
>示例

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np

s=Series([1,2,3,np.nan])
print s
#使用一个指定的指来替换缺失的值,这会产生一个新的对像
s1=s.fillna(1)
print '替换后产生新对象值\r\n',s1
print '原有对象S并没有产生变化\r\n',s
s2=s.fillna(1,inplace=True)
print '使用inplace=True后原有对象S产生变化\r\n',s
```
>输出

```
0     1
1     2
2     3
3   NaN
dtype: float64
替换后产生新对象值
0    1
1    2
2    3
3    1
dtype: float64
原有对象S并没有产生变化
0     1
1     2
2     3
3   NaN
dtype: float64
使用inplace=True后原有对象S产生变化
0    1
1    2
2    3
3    1
```
#滤除缺失数据#

<Strong>对于Series和DataFrame都可以使用方法dropna()滤除缺失数据，且对于DataFrame还可以指定axis参数来滤除指定轴上的缺失数据；通常是将有缺失值的行或列去掉</Strong>

