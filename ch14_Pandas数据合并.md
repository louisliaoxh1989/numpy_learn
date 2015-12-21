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

4. 









#注解#

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
