#merage#

<em>pandas提供了一个类似于关系数据库的连接(join)操作的方法<Strong>merage</Strong>,可以根据一个或多个键将不同DataFrame中的行连接起来</em>

<em>语法如下</em>

```python
merge(left, right, how='inner', on=None, left_on=None, right_on=None,
      left_index=False, right_index=False, sort=True,
      suffixes=('_x', '_y'), copy=True, indicator=False)
```
