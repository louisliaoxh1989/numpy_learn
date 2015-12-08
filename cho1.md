#一、基本概念
***1. NumPy中的 ndarray 是一个多维数组对象，该对象由两部分组成实际的数据以及描述这些数据的元数据,大部分的数组操作仅仅修改元数据部分，而不改变底层的实际数据***
***2. NumPy数组一般是同质的（但有一种特殊的数组类型例外，它是异质的）,即所有元素的类型必须一致***
***3. Numpy数组下标是从0开始的***

#二、创建一维数组

>>array([元素1、元素2、....、元素N])
```python
# coding=utf-8
import numpy as np
arr1 = np.array([1,2,3,4])
```
> [1 2 3 4]

*可以使用arrange(n)生成0(包含)到n(不包含)的数字的 一维数组(ndarray)*
```python
# coding=utf-8
import numpy as np
arr1=np.arange(10)
type(arr1)
```
> [0 1 2 3 4 5 6 7 8 9]
>type 'numpy.ndarray' 

#三、创建多维数组

创建一个3*3的数组
``` python
# coding = utf-8
import numpy as np
arr1 = np.array([np.arange(3),np.arange(3),np.arange(3)])
```
>[[0 1 2]
 [0 1 2]
 [0 1 2]]

``` python
# coding = utf-8
import numpy as np
arr1 = np.array([["a","b","c"],["d","e","f"],["g","h","i"]])
```
>[
   ['a' 'b' 'c']
	   ['d' 'e' 'f']
	   ['g' 'h' 'i']
>]

#四、更换数组维度
***1.通过修改数组的shape属性，在保持数组元素个数不变的情况下，改变数组每个轴的长度***
```python
# coding=utf-8
import numpy as np
arr1=np.arange(10)
print arr1
#变成2*5的数组
arr1.shape=2,5 
print arr1
```
>[0 1 2 3 4 5 6 7 8 9]
>[[0 1 2 3 4]
 [5 6 7 8 9]]

***2.使用数组的reshape方法，可以创建一个改变了尺寸的新数组，原数组的shape保持不变***

```python
# coding=utf-8
import numpy as np
arr1=np.arange(10)
print arr1
#生成一个新2*5的数组，原数组不为变
arr1.reshape(2,5)
print arr1
arr2 = arr1.reshape(2,5)
print arr2
#数组arry1和arry2其实共享数据存储内存区域，因此修改其中任意一个数组的元素都会同时修改另外一个数组的内容
arr1[0]= 10
print arr2
print arr1
```
>[0 1 2 3 4 5 6 7 8 9]
[0 1 2 3 4 5 6 7 8 9]
[
 [10  1  2  3  4]
 [ 5  6  7  8  9]
 ]
[10  1  2  3  4  5  6  7  8  9]

***3. 使用flatten或ravel展平数组***
这两个方法都不会改变原有数组

```python
# coding=utf-8
import numpy as np
arr1=np.arange(10).reshape(2,5)
print arr1
#用 ravel 函数来展平(转成一维数组）
print arr1.ravel()
#原有数组arr1并没有变化
print arr1
#也可使用 flatten,但 flatten 函数会请求分配内存来保存结果,而ravle只是用来展现视图
print arr1.flatten()
#同样原有数组arr1也没有变化
print arr1
```
>[[0 1 2 3 4]
 [5 6 7 8 9]]
[0 1 2 3 4 5 6 7 8 9]
[[0 1 2 3 4]
 [5 6 7 8 9]]
[0 1 2 3 4 5 6 7 8 9]
[[0 1 2 3 4]
 [5 6 7 8 9]]

***4.使用transpose方法来转置矩阵***
```python
# coding=utf-8
import numpy as np
arr1=np.arange(10).reshape(2,5)
print arr1
print arr1.transpose()
```
>[[0 1 2 3 4]
 [5 6 7 8 9]]
[[0 5]
 [1 6]
 [2 7]
 [3 8]
 [4 9]]
#五、获取数组元素类型

使用数组的dtype属性可获取到ndarray数组的元素类型

```python
# coding=utf-8
import numpy as np
arr1 = np.arange(10)
print arr1.dtype
```
>int32 #如果是64位机器则是int64

#六、获取数组维度
使用数组的shape属性可获取到ndarray数组的维度(大小)。数组的 shape 属性返回一个元组（tuple） ，元组中的元素即为NumPy数组每一个维度上的大小。
```python
# coding=utf-8
import numpy as np
arr1 = np.arange(10).reshape(2,5)
print arr1.shape
```
>(2,5)


