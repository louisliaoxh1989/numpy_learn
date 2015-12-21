<B>[merage](#merage)</B>

<em>&nbsp;&nbsp;[内连接](#内连接)</em>

<em>&nbsp;&nbsp;[左外连接](#左外连接)</em>

<em>&nbsp;&nbsp;[右外连接](#右外连接)</em>

<em>&nbsp;&nbsp;[全外连接](#全外连接)</em>

<em>&nbsp;&nbsp;[示例](#示例)</em>

<B>[join](#join)</B>

<B>[concat](#concat)</B>

#merage#

<em>pandas提供了一个类似于关系数据库的连接(join)操作的方法<Strong>merage</Strong>,可以根据一个或多个键将不同DataFrame中的行连接起来</em>

<em>语法如下</em>

```python
merge(left, right, how='inner', on=None, left_on=None, right_on=None,
      left_index=False, right_index=False, sort=True,
      suffixes=('_x', '_y'), copy=True, indicator=False)
```

>参数说明：

1. left与right：两个不同的DataFrame
2. how：指的是合并(连接)的方式有[inner(内连接)](#内连接),[left(左外连接)](#左外连接),[right(右外连接)](#右外连接),[outer(全外连接)](#全外连接);默认为inner
3. on : 指的是用于连接的列索引名称。必须存在右右两个DataFrame对象中，如果没有指定且其他参数也未指定则以两个DataFrame的列名交集做为连接键
4. left_on：左则DataFrame中用作连接键的列名;这个参数中左右列名不相同，但代表的含义相同时非常有用。
5. right_on：右则DataFrame中用作 连接键的列名
6. left_index：使用左则DataFrame中的行索引做为连接键
7. right_index：使用右则DataFrame中的行索引做为连接键
8. sort：默认为True，将合并的数据进行排序。在大多数情况下设置为False可以提高性能
9. suffixes：字符串值组成的元组，用于指定当左右DataFrame存在相同列名时在列名后面附加的后缀名称，默认为('_x','_y')
10. copy：默认为True,总是将数据复制到数据结构中；大多数情况下设置为False可以提高性能
11. indicator：在 0.17.0中还增加了一个显示合并数据中来源情况；如只来自己于左边(left_only)、两者(both)



##内连接##

<em>符合连接条件和查询条件的数据行，相当于数据库中的jion,示例SQL语句 </em>

```sql
SELECT *
FROM df1
INNER JOIN df2
  ON df1.key = df2.key;
```
或
```sql
SELECT *
FROM df1,df2 where df1.key=df2.key
```

<em>对应pandas语句</em>

```python
pd.merge(df1, df2, on='key')
```


##左外连接##

<em>符合连接条件和查询条件的数据行并返回左表中不符合连接条件单符合查询条件的数据行，相当于数据库中的left outer join,示例SQL语句 </em>
```sql
-- show all records from df1
SELECT *
FROM df1
LEFT OUTER JOIN df2
  ON df1.key = df2.key;
```

<em>对应pandas语句</em>

```python
pd.merge(df1, df2, on='key', how='left')
```

##右外连接##

<em>符合连接条件和查询条件的数据行并返回右表中不符合连接条件单符合查询条件的数据行，相当于数据库中的right outer join,示例SQL语句 </em>

```sql
-- show all records from df2
SELECT *
FROM df1
RIGHT OUTER JOIN df2
  ON df1.key = df2.key;
```
<em>对应pandas语句</em>

```python
pd.merge(df1, df2, on='key', how='right')
```


##全外连接##

<em>符合连接条件和查询条件的数据行并返回左表和左表中不符合连接条件单符合查询条件的数据行。全外连接相当于左外连接与左外连接的合集(去掉重复),相当于数据库中的full outer join,示例SQL语句 </em>

```sql
-- show all records from both tables
SELECT *
FROM df1
FULL OUTER JOIN df2
  ON df1.key = df2.key;
```
<em>对应pandas语句</em>

```python
pd.merge(df1, df2, on='key', how='outer')
```
***更详细的参见[官方网站](http://pandas.pydata.org/pandas-docs/stable/comparison_with_sql.html#compare-with-sql-join)***


##示例##
```python
#coding=utf-8
from pandas import Series,DataFrame,merge
import numpy as np
data=DataFrame([{"id":0,"name":'lxh',"age":20,"cp":'lm'},{"id":1,"name":'xiao',"age":40,"cp":'ly'},{"id":2,"name":'hua',"age":4,"cp":'yry'},{"id":3,"name":'be',"age":70,"cp":'old'}])
data1=DataFrame([{"id":100,"name":'lxh','cs':10},{"id":101,"name":'xiao','cs':40},{"id":102,"name":'hua2','cs':50}])
data2=DataFrame([{"id":0,"name":'lxh','cs':10},{"id":101,"name":'xiao','cs':40},{"id":102,"name":'hua2','cs':50}])

print "单个列名做为内链接的连接键\r\n",merge(data,data1,on="name",suffixes=('_a','_b'))
print "多列名做为内链接的连接键\r\n",merge(data,data2,on=("name","id"))
print '不指定on则以两个DataFrame的列名交集做为连接键\r\n',merge(data,data2) #这里使用了id与name

#使用右边的DataFrame的行索引做为连接键
##设置行索引名称
indexed_data1=data1.set_index("name")
print "使用右边的DataFrame的行索引做为连接键\r\n",merge(data,indexed_data1,left_on='name',right_index=True)


print '左外连接\r\n',merge(data,data1,on="name",how="left",suffixes=('_a','_b'))
print '左外连接1\r\n',merge(data1,data,on="name",how="left")
print '右外连接\r\n',merge(data,data1,on="name",how="right")
data3=DataFrame([{"mid":0,"mname":'lxh','cs':10},{"mid":101,"mname":'xiao','cs':40},{"mid":102,"mname":'hua2','cs':50}])

#当左右两个DataFrame的列名不同，当又想做为连接键时可以使用left_on与right_on来指定连接键
print "使用left_on与right_on来指定列名字不同的连接键\r\n",merge(data,data3,left_on=["name","id"],right_on=["mname","mid"])
```
>输出 

```python
单个列名做为内链接的连接键
   age  cp  id_a  name  cs  id_b
0   20  lm     0   lxh  10   100
1   40  ly     1  xiao  40   101
多列名做为内链接的连接键
   age  cp  id name  cs
0   20  lm   0  lxh  10
不指定on则以两个DataFrame的列名交集做为连接键
   age  cp  id name  cs
0   20  lm   0  lxh  10
使用右边的DataFrame的行索引做为连接键
   age  cp  id_x  name  cs  id_y
0   20  lm     0   lxh  10   100
1   40  ly     1  xiao  40   101
左外连接
   age   cp  id_a  name  cs  id_b
0   20   lm     0   lxh  10   100
1   40   ly     1  xiao  40   101
2    4  yry     2   hua NaN   NaN
3   70  old     3    be NaN   NaN
左外连接1
   cs  id_x  name  age   cp  id_y
0  10   100   lxh   20   lm     0
1  40   101  xiao   40   ly     1
2  50   102  hua2  NaN  NaN   NaN
右外连接
   age   cp  id_x  name  cs  id_y
0   20   lm     0   lxh  10   100
1   40   ly     1  xiao  40   101
2  NaN  NaN   NaN  hua2  50   102
使用left_on与right_on来指定列名字不同的连接键
   age  cp  id name  cs  mid mname
0   20  lm   0  lxh  10    0   lxh
```

#join#

<em>join方法提供了一个简便的方法用于将两个DataFrame中的不同的列索引合并成为一个DataFrame</em>

```python
join(self, other, on=None, how='left', lsuffix='', rsuffix='',
             sort=False):
```

*其中参数的意义与merge方法基本相同,只是join方法默认为左外连接how=left*

>示例

```python
#coding=utf-8
from pandas import Series,DataFrame,merge

data=DataFrame([{"id":0,"name":'lxh',"age":20,"cp":'lm'},{"id":1,"name":'xiao',"age":40,"cp":'ly'},{"id":2,"name":'hua',"age":4,"cp":'yry'},{"id":3,"name":'be',"age":70,"cp":'old'}],index=['a','b','c','d'])
data1=DataFrame([{"sex":0},{"sex":1},{"sex":2}],index=['a','b','e'])

print '使用默认的左连接\r\n',data.join(data1)  #这里可以看出自动屏蔽了data中没有的index=e 那一行的数据
print '使用右连接\r\n',data.join(data1,how="right") #这里出自动屏蔽了data1中没有index=c,d的那行数据；等价于data1.join(data)
print '使用内连接\r\n',data.join(data1,how='inner')
print '使用全外连接\r\n',data.join(data1,how='outer')
```
>输出
```python
使用默认的左连接
   age   cp  id  name  sex
a   20   lm   0   lxh    0
b   40   ly   1  xiao    1
c    4  yry   2   hua  NaN
d   70  old   3    be  NaN
使用右连接
   age   cp  id  name  sex
a   20   lm   0   lxh    0
b   40   ly   1  xiao    1
e  NaN  NaN NaN   NaN    2
使用内连接
   age  cp  id  name  sex
a   20  lm   0   lxh    0
b   40  ly   1  xiao    1
使用全外连接
   age   cp  id  name  sex
a   20   lm   0   lxh    0
b   40   ly   1  xiao    1
c    4  yry   2   hua  NaN
d   70  old   3    be  NaN
e  NaN  NaN NaN   NaN    2
```

#concat#

<em>concat方法相当于数据库中的全连接(UNION ALL),可以指定按某个轴进行连接,也可以指定连接的方式join(outer,inner 只有这两种)。***与数据库不同的时concat不会去重，要达到去重的效果可以使用drop_duplicates方法***</em>

```python
 concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False,
           keys=None, levels=None, names=None, verify_integrity=False, copy=True):
```

>参数说明

Parameters

    ----------
    objs : a sequence or mapping of Series, DataFrame, or Panel objects
        If a dict is passed, the sorted keys will be used as the `keys`
        argument, unless it is passed, in which case the values will be
        selected (see below). Any None objects will be dropped silently unless
        they are all None in which case a ValueError will be raised
    axis : {0, 1, ...}, default 0
        The axis to concatenate along
    join : {'inner', 'outer'}, default 'outer'
        How to handle indexes on other axis(es)
    join_axes : list of Index objects
        Specific indexes to use for the other n - 1 axes instead of performing
        inner/outer set logic
    verify_integrity : boolean, default False
        Check whether the new concatenated axis contains duplicates. This can
        be very expensive relative to the actual data concatenation
    keys : sequence, default None
        If multiple levels passed, should contain tuples. Construct
        hierarchical index using the passed keys as the outermost level
    levels : list of sequences, default None
        Specific levels (unique values) to use for constructing a
        MultiIndex. Otherwise they will be inferred from the keys
    names : list, default None
        Names for the levels in the resulting hierarchical index
    ignore_index : boolean, default False
        If True, do not use the index values along the concatenation axis. The
        resulting axis will be labeled 0, ..., n - 1. This is useful if you are
        concatenating objects where the concatenation axis does not have
        meaningful indexing information. Note the the index values on the other
        axes are still respected in the join.
    copy : boolean, default True
        If False, do not copy data unnecessarily


>示例

```python
#coding=utf-8
from pandas import Series,DataFrame,concat

df1 = DataFrame({'city': ['Chicago', 'San Francisco', 'New York City'], 'rank': range(1, 4)})
df2 = DataFrame({'city': ['Chicago', 'Boston', 'Los Angeles'], 'rank': [1, 4, 5]})
print '按轴进行内连接\r\n',concat([df1,df2],join="inner",axis=1)
print '进行外连接并指定keys(行索引)\r\n',concat([df1,df2],keys=['a','b']) #这里有重复的数据
print '去重后\r\n',concat([df1,df2],ignore_index=True).drop_duplicates()

```

>输出
```python
按轴进行内连接
            city  rank         city  rank
0        Chicago     1      Chicago     1
1  San Francisco     2       Boston     4
2  New York City     3  Los Angeles     5
进行外连接并指定keys(行索引)
              city  rank
a 0        Chicago     1
  1  San Francisco     2
  2  New York City     3
b 0        Chicago     1
  1         Boston     4
  2    Los Angeles     5
去重后
            city  rank
0        Chicago     1
1  San Francisco     2
2  New York City     3
4         Boston     4
5    Los Angeles     5

```
