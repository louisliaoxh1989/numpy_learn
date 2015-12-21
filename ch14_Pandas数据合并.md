<B>[merage](#merage)</B>

<em>&nbsp;&nbsp;[内连接](#内连接)</em>

<em>&nbsp;&nbsp;[左外连接](#左外连接)</em>

<em>&nbsp;&nbsp;[右外连接](#右外连接)</em>

<em>&nbsp;&nbsp;[全外连接](#全外连接)</em>

<em>&nbsp;&nbsp;[示例](#示例)</em>


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
