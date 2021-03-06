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

#排序操作#

<em>对于Series若要对值进行排序可以使用<Strong>order(ascending=True or False)</Strong>方法来进行升序或降序排列</em>

<em>对于DataFrame若要对值进行排序可以使用<Strong>sort(columns=['one','three'],ascending=False)</Strong>方法，按照一列或多个列的值来进行升序或降序排列</em>

<em>对于DataFrame若要对列或行的索引值进行排序可以使用<Strong>sort_index(axis=0或1,ascending=False)</Strong>方法对DataFrame按列或行的索引值升序或降序排列</em>

>示例

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
df1 = DataFrame({'one':Series(np.array([1,-2.2,1,2]),index=['a', 'b', 'c','d']), 'two' : Series(np.array([2,4.2,-3.25,-5]), index=['a', 'b', 'c', 'd']),'three':Series(np.array([2,2,4.2,-3.25]), index=['a','b', 'c', 'd'])},columns=['one','two','three'])
print '原数据\r\n',df1
#排序sort与order
print '先以列one然后按列three的值进行升序排列\r\n',df1.sort(columns=['one','three'])
print '先以列one然后按列three的值进行降序序排列\r\n',df1.sort(columns=['one','three'],ascending=False)
print '在列索引的值按字典升序排列\r\n',df1.sort_index(axis=1)
print '在列索引的值按字典降序排列\r\n',df1.sort_index(axis=1,ascending=False)
#对Series，进行按值排序
s=Series([1,-20,30,10],index=['a','b','c','d'])
print '对Series进行按值升序排序\r\n',s.order()
```
>输出

```
原数据
   one   two  three
a  1.0  2.00   2.00
b -2.2  4.20   2.00
c  1.0 -3.25   4.20
d  2.0 -5.00  -3.25
先以列one然后按列three的值进行升序排列
   one   two  three
b -2.2  4.20   2.00
a  1.0  2.00   2.00
c  1.0 -3.25   4.20
d  2.0 -5.00  -3.25
先以列one然后按列three的值进行降序序排列
   one   two  three
d  2.0 -5.00  -3.25
c  1.0 -3.25   4.20
a  1.0  2.00   2.00
b -2.2  4.20   2.00
在列索引的值按字典升序排列
   one  three   two
a  1.0   2.00  2.00
b -2.2   2.00  4.20
c  1.0   4.20 -3.25
d  2.0  -3.25 -5.00
在列索引的值按字典降序排列
    two  three  one
a  2.00   2.00  1.0
b  4.20   2.00 -2.2
c -3.25   4.20  1.0
d -5.00  -3.25  2.0
对Series进行按值升序排序
b   -20
a     1
d    10
c    30
dtype: int64
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

#函数应用与映射#

<em>有的情况下，需要将自己定义的函数或第三方函数应用到DataFrame的每行或每列上最张形成一个一维数组，这就需要用到<Strong>apply(func,axis)</Strong>方法</em>


<em>有的情况下，需要将值中的数据格式进行转换如想得到每个值的格式化字符串，这就需要用到<Strong>applymap(func,axis)</Strong>方法</em>

<em>更详细的参见[官方网站](http://pandas.pydata.org/pandas-docs/stable/basics.html#function-application)</em>


>示例1：计算每列上最大值与最小值之间的差值

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
import pandas as pd
df1 = DataFrame({'one':Series(np.array([1,-2.2,3.45]),index=['a', 'b', 'c']), 'two' : Series(np.array([2,4.2,-3.25,-5]), index=['a', 'b', 'c', 'd']),'three':Series(np.array([2,4.2,-3.25]), index=['b', 'c', 'd'])},columns=['one','two','three'])
print '原数据\r\n',df1
#最大值减去最小值
f=lambda x: x.max() - x.min()
#在列索引中的每一列上计算出该列中最大值与早小值之间的差值
print '各列上最大值与早小值之间的差值\r\n',df1.apply(f)
#格式化每个值
f1=lambda  x: '%.3f'%x
print '格式化每个值\r\n',df1.applymap(f1)
```
>输出

```
原数据
    one   two  three
a  1.00  2.00    NaN
b -2.20  4.20   2.00
c  3.45 -3.25   4.20
d   NaN -5.00  -3.25
各列上最大值与早小值之间的差值
one      5.65
two      9.20
three    7.45
dtype: float64
格式化每个值
      one     two   three
a   1.000   2.000     nan
b  -2.200   4.200   2.000
c   3.450  -3.250   4.200
d     nan  -5.000  -3.250
```





#聚合功能#

##Series唯一值及值计数##

<em>使用Series对象的unique()可以将重复的元素去掉;使用Pandas的value_counts()则可以计算每个元素出现的次数</em>

>示例

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
import pandas as pd
#唯一值及值计数
s1=Series(['user3','user1','user3','user2','user3','user1'])
#去除重复的元素
uniques=s1.unique()
print '原有的Series\r\n',s1
print '去重后的Series并按元素排列\r\n',np.sort(uniques)
print '每个值及值出现的次数',s1.value_counts().to_dict()
print '每个值及值出现的次数并按出现的次数排序\r\n',pd.value_counts(s1.values,sort=True).to_json()
```
>输出
```
原有的Series
0    user3
1    user1
2    user3
3    user2
4    user3
5    user1
dtype: object
去重后的Series并按元素排列
['user1' 'user2' 'user3']
每个值及值出现的次数 {'user2': 1, 'user3': 3, 'user1': 2}
每个值及值出现的次数并按出现的次数排序
{"user3":3,"user1":2,"user2":1}
```

##汇总和计算描述统计##

###汇总###

<table border="1" class="docutils">
<colgroup>
<col width="20%">
<col width="80%">
</colgroup>
<thead valign="bottom">
<tr class="row-odd"><th class="head">Function</th>
<th class="head">Description</th>
</tr>
</thead>
<tbody valign="top">
<tr class="row-even"><td><tt class="docutils literal"><span class="pre">count</span></tt></td>
<td>Number of non-null observations统计非NA值的个数</td>
</tr>
<tr class="row-odd"><td><tt class="docutils literal"><span class="pre">sum</span></tt></td>
<td>对某一轴上(axis=1或0)的值求合</td>
</tr>
<tr class="row-even"><td><tt class="docutils literal"><span class="pre">mean</span></tt></td>
<td>对某一轴上(axis=1或0)的值求平均</td>
</tr>
<tr class="row-odd"><td><tt class="docutils literal"><span class="pre">mad</span></tt></td>
<td>根据平均值平均计算绝对离差</td>
</tr>
<tr class="row-even"><td><tt class="docutils literal"><span class="pre">median</span></tt></td>
<td>值的算术中位数</td>
</tr>
<tr class="row-odd"><td><tt class="docutils literal"><span class="pre">min</span></tt></td>
<td>某一轴上(axis=1或0)上的最小值</td>
</tr>
<tr class="row-even"><td><tt class="docutils literal"><span class="pre">max</span></tt></td>
<td>某一轴上(axis=1或0)上的最大值</td>
</tr>
<tr class="row-odd"><td><tt class="docutils literal"><span class="pre">mode</span></tt></td>
<td>除法</td>
</tr>
<tr class="row-even"><td><tt class="docutils literal"><span class="pre">abs</span></tt></td>
<td>绝对值</td>
</tr>
<tr class="row-odd"><td><tt class="docutils literal"><span class="pre">prod</span></tt></td>
<td>Product of values</td>
</tr>
<tr class="row-even"><td><tt class="docutils literal"><span class="pre">std</span></tt></td>
<td>标准差</td>
</tr>
<tr class="row-odd"><td><tt class="docutils literal"><span class="pre">var</span></tt></td>
<td>方差</td>
</tr>
<tr class="row-even"><td><tt class="docutils literal"><span class="pre">sem</span></tt></td>
<td>平均数标准误差</td>
</tr>
<tr class="row-odd"><td><tt class="docutils literal"><span class="pre">skew</span></tt></td>
<td>偏度 (3rd moment三阶矩)</td>
</tr>
<tr class="row-even"><td><tt class="docutils literal"><span class="pre">kurt</span></tt></td>
<td>峰度 (4th moment四阶矩)</td>
</tr>
<tr class="row-odd"><td><tt class="docutils literal"><span class="pre">quantile</span></tt></td>
<td>分位数(value at %)</td>
</tr>
<tr class="row-even"><td><tt class="docutils literal"><span class="pre">cumsum</span></tt></td>
<td>累计和</td>
</tr>
<tr class="row-odd"><td><tt class="docutils literal"><span class="pre">cumprod</span></tt></td>
<td>累计积</td>
</tr>
<tr class="row-even"><td><tt class="docutils literal"><span class="pre">cummax</span></tt></td>
<td>累计最大值</td>
</tr>
<tr class="row-odd"><td><tt class="docutils literal"><span class="pre">cummin</span></tt></td>
<td>累计最小值</td>
</tr>
</tbody>
</table>

<em>Series和DataFrame都可以使用上面的方法，对于DataFrame可以指定axis来进行某一个轴(行或列)上的操作</em>

>示例

```python
#coding=utf-8
from pandas import Series,DataFrame
df1 = DataFrame({'one':Series(np.array([1,-2.2,3.45]),index=['a', 'b', 'c']), 'two' : Series(np.array([2,4.2,-3.25,-5]), index=['a', 'b', 'c', 'd']),'three':Series(np.array([2,4.2,-3.25]), index=['b', 'c', 'd'])},columns=['one','two','three'])
print '原数据\r\n',df1
##求合sum
print "按列进行求合\r\n",df1.sum(axis=0)
print "按行进行求合\r\n",df1.sum(axis=1)
##求平均
print "按列进行平均\r\n",df1.mean(axis=0)
print "按行进行平均\r\n",df1.mean(axis=1)
#取最大值
print '按列取最大值\r\n',df1.max()
print '按行取最大值\r\n',df1.max(axis=1)
#非NA的个数
print '按列查看非NA的个数\r\n',df1.count().to_dict()
print '按列查看非NA的个数\r\n',df1.count(axis=1).to_dict()
```
>输出 

```
原数据
    one   two  three
a  1.00  2.00    NaN
b -2.20  4.20   2.00
c  3.45 -3.25   4.20
d   NaN -5.00  -3.25
按列进行求合
one      2.25
two     -2.05
three    2.95
dtype: float64
按行进行求合
a    3.00
b    4.00
c    4.40
d   -8.25
dtype: float64
按列进行平均
one      0.750000
two     -0.512500
three    0.983333
dtype: float64
按行进行平均
a    1.500000
b    1.333333
c    1.466667
d   -4.125000
dtype: float64
按列取最大值
one      3.45
two      4.20
three    4.20
dtype: float64
按行取最大值
a    2.00
b    4.20
c    4.20
d   -3.25
dtype: float64
按列查看非NA的个数
{'three': 3, 'two': 4, 'one': 3}
按列查看非NA的个数
{'a': 2, 'c': 3, 'b': 3, 'd': 2}
```
###计算描述统计###

<em>使用<Strong>describe</Strong>可以一次性的产生多种数值统计如count,mean,max,min</em>

>示例
```python
#coding=utf-8
from pandas import Series,DataFrame
df1 = DataFrame({'one':Series(np.array([1,-2.2,3.45]),index=['a', 'b', 'c']), 'two' : Series(np.array([2,4.2,-3.25,-5]), index=['a', 'b', 'c', 'd']),'three':Series(np.array([2,4.2,-3.25]), index=['b', 'c', 'd'])},columns=['one','two','three'])
print '原数据\r\n',df1
print '计算描述统计\r\n',df1.describe()
```
>输出

```
原数据
    one   two  three
a  1.00  2.00    NaN
b -2.20  4.20   2.00
c  3.45 -3.25   4.20
d   NaN -5.00  -3.25
计算描述统计
            one       two     three
count  3.000000  4.000000  3.000000
mean   0.750000 -0.512500  0.983333
std    2.833284  4.326349  3.827641
min   -2.200000 -5.000000 -3.250000
25%   -0.600000 -3.687500 -0.625000
50%    1.000000 -0.625000  2.000000
75%    2.225000  2.550000  3.100000
max    3.450000  4.200000  4.200000
```
