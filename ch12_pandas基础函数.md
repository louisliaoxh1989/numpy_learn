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

#head与tail

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
