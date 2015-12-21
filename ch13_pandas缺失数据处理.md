
#查看数据中那些有缺失值NaN#

<Strong>对于Series和DataFrame都可以使用方法isNull<Strong>

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

