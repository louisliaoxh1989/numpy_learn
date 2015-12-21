#merage#

<em>pandas提供了一个类似于关系数据库的连接(join)操作的方法<Strong>merage</Strong>,可以根据一个或多个键将不同DataFrame中的行连接起来</em>

<em>语法如下</em>

```python
merge(left, right, how='inner', on=None, left_on=None, right_on=None,
      left_index=False, right_index=False, sort=True,
      suffixes=('_x', '_y'), copy=True, indicator=False)
```

```
参数说明：

1. left与right：两个不同的DataFrame

2. how：指的是合并(连接)的方式，有[inner(内连接)],outer(外连接),left(左连接),right(右连接);默认为inner

3. on : 指的是用于连接的列索引名称。必须存在右右两个DataFrame对象中，如果没有指定且其他参数也未指定则以两个DataFrame的列名交集做为连接键

4. 

```








#注解#

##内连接##

<em>符合连接条件和查询条件的数据行，相当于数据库中的SQL语句 </em>

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
