<strong>目录</Strong>

<B>[通过布尔向量选择过虑数据](#通过布尔向量选择过虑数据)</B>

<B>[通过isin](#通过isin)</B>

<B>[使用query](#使用query)</B>

#通过布尔向量选择过虑数据#

<em>当需要使用多个条件组合时可以使用操作符 "|"、"&"、"~" 他们分别代表"或"、"与"以及"非",且每个条件都使用括号包括起来</em>

##对Series使用布尔向量选择数据##

>示例：

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
#对于Series数据使用布尔向量过滤数据
l=[-1, 2, 3, 10, 20, -20, 90]
s=Series(l)
print '选择所有大于0的数据\r\n',s[s>0].tolist()
print '选择所有大于0且小于20的数据\r\n',s[(s>0) & (s<20)].tolist()
print '选择所有小于0或大于20的数据\r\n',s[(s<0) | (s>20)].tolist()
print '选择所有不大于0的数据\r\n',s[~(s>0)].tolist()
```
>输出

```
选择所有大于0的数据
[2, 3, 10, 20, 90]
选择所有大于0且小于20的数据
[2, 3, 10]
选择所有小于0或大于20的数据
[-1, -20, 90]
选择所有不大于0的数据
[-1, -20]
```

##对DataFrame使用布尔向量选择数据##

>示例

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
data=[{"name":'lxh',"age":20,"cp":'lm'},{"name":'xiao',"age":40,"cp":'ly'},{"name":'hua',"age":4,"cp":'yry'},{"name":'be',"age":70,"cp":'old'}]
df=DataFrame(data,columns=['name','cp','age'])
print '原数据\r\n',df

#简单的
print '所有age大于20的数据\r\n',df[(df["age"]>20)]
print '所有age大于20且小于70的数据\r\n',df[(df["age"]>20) & (df["age"]<70)]

#复杂的多条件

#这里使用map将会提升过滤速度
criterion = df['name'].map(lambda x: (x.startswith('x') | x.startswith('l')))
print '所有name以字母x或l开头的age数据\r\n',df[criterion]['age']
#多列过滤
print '所有name以字母x或l开头且age小于40的数据\r\n',df[criterion & (df['age']<40)]

```
>输出 

```
原数据
   name   cp  age
0   lxh   lm   20
1  xiao   ly   40
2   hua  yry    4
3    be  old   70
所有age大于20的数据
   name   cp  age
1  xiao   ly   40
3    be  old   70
所有age大于20且小于70的数据
   name  cp  age
1  xiao  ly   40
所有name以字母x或l开头的age数据
0    20
1    40
Name: age, dtype: int64
所有name以字母x或l开头且age小于40的数据
  name  cp  age
0  lxh  lm   20
```

#通过isin(values)#

>示例

```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
data=[{"name":'lxh',"age":20,"cp":'lm'},{"name":'xiao',"age":40,"cp":'ly'},{"name":'hua',"age":4,"cp":'yry'},{"name":'be',"age":70,"cp":'old'}]
df=DataFrame(data,columns=['name','cp','age'])
print '原数据\r\n',df
#使用isin(values)
values={'name':['lxh','xiao']}
#取得每行对应的布尔值
index=df.isin(values).any(1)
print '取得每行对应的布尔值\r\n',index
print '选择name在[lxh，xiao]中的数据\r\n',df[index]
```
>输出 

```
原数据
   name   cp  age
0   lxh   lm   20
1  xiao   ly   40
2   hua  yry    4
3    be  old   70
取得每行对应的布尔值
0     True
1     True
2    False
3    False
dtype: bool
选择name在[lxh，xiao]中的数据
   name  cp  age
0   lxh  lm   20
1  xiao  ly   40
```

#使用query#

<em>query方法允许使用表达式的方法来选取数据；如你想选择列索引(标签)为"age"的大于10小于40的数据则可以使用</em>

query("10<age<40")
```python
#coding=utf-8
from pandas import Series,DataFrame
import numpy as np
data=[{"name":'lxh',"age":20,"cp":'lm'},{"name":'xiao',"age":40,"cp":'ly'},{"name":'hua',"age":4,"cp":'yry'},{"name":'be',"age":70,"cp":'old'}]
df=DataFrame(data,columns=['name','cp','age'])
print '原数据\r\n',df
#使用query#
print '所有age大于10且小于40的数据\r\n',df.query('10<age<40')

print '选择name在[lxh，hua]中的数据\r\n',df.query('name==["lxh","hua"]')

print '选择name在[lxh，hua]中的数据方法二\r\n',df.query('["lxh","hua"] in name')

print '选择name不在[lxh，hua]中的数据\r\n',df.query('["lxh","hua"] not in name')

print '选择name在[lxh，hua]中且age大于10的数据\r\n',df.query('name==["lxh","hua"] and age>10')

print '选择name在[lxh，hua]中或age大于10的数据\r\n',df.query('name==["lxh","hua"] or age>10')
```
>输出 
```
原数据
   name   cp  age
0   lxh   lm   20
1  xiao   ly   40
2   hua  yry    4
3    be  old   70
所有age大于10且小于40的数据
  name  cp  age
0  lxh  lm   20
选择name在[lxh，hua]中的数据
  name   cp  age
0  lxh   lm   20
2  hua  yry    4
选择name在[lxh，hua]中的数据方法二
  name   cp  age
0  lxh   lm   20
2  hua  yry    4
选择name不在[lxh，hua]中的数据
   name   cp  age
1  xiao   ly   40
3    be  old   70
选择name在[lxh，hua]中且age大于10的数据
  name  cp  age
0  lxh  lm   20
选择name在[lxh，hua]中或age大于10的数据
   name   cp  age
0   lxh   lm   20
1  xiao   ly   40
2   hua  yry    4
3    be  old   70
```
