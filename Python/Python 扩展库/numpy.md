NumPy
---
1. 有ndarray对象和ufunc函数
2. 具有精巧的函数
3. 比较适合线性代数和随机数处理等科学计算
4. 有效的通用多维数据，可以定义任意数据类型
5. 无缝对接数据库



# 1. 函数
1. 首先导入:`import numpy as np`
2. `np.ones((m,n))`创建一个m*n的多维数组，元素值为1
3. `np.linspace(a,b,number)`生成从a到b的number个等差数据
4. `np.sin(x)`等三角变换
5. `np.fft.fft(list)`进行快速傅里叶变换
    + np.fft是一个库
6. `np.tile(A,reps)`:将A按照reps进行重复可以是简单重复，也可以是数组形式重复
```py
tile(1,2)
# array([1, 1])
tile((1,2,3),3)
# array([1, 2, 3, 1, 2, 3, 1, 2, 3])
tile(a,2)
# array([[1, 2, 3, 1, 2, 3],
#       [4, 5, 5, 4, 5, 5]])
b=[1,3,5]
tile(b,[2,3])
# array([[1, 3, 5, 1, 3, 5, 1, 3, 5],
#        [1, 3, 5, 1, 3, 5, 1, 3, 5]])
 
a=[[1,2,3],[5,4]]
tile(a,[2,3])
# array([[[1, 2, 3], [5, 4], [1, 2, 3], [5, 4], [1, 2, 3], [5, 4]],
#        [[1, 2, 3], [5, 4], [1, 2, 3], [5, 4], [1, 2, 3], [5, 4]]])
```

# 2. ndarray对象
1. list和tuple和array不同，同时array不是python的内置模块，函数不多
2. 是NumPy的基本的数据结构
3. 别名是array，利于节省内存和提高CPU计算时间，并有丰富的函数

## 2.1. 数组属性
1. 维度:轴，轴的个数:秩
2. 基本属性
    1. ndarray.ndim:秩
    2. ndarray.shape:维度
    3. ndarray.size:元素总个数
    4. ndarray.dtype:元素类型
    5. ndarray.itemsize:元素字节大小

## 2.2. 创建方法
1. 基本:`array(list)`
2. `np.arange()`可以处理浮点数
3. `np.random.random()`
4. `np.linspace(start,end,number,endpoint = T/F)`从起始点到终止点在个数确定的时候，确定一个等差数组
5. `np.ones(list)`
6. `np.zeros(list)`
7. `np.fromfunction(lambda i,j:(i+1)*(j+1),(9,9))`用函数来创建某一个维度的数组
8. `np.array_split(a,b)`:将链表a中元素生成成为一个个长度不大于b的数组

## 2.3. 操作方法
1. 切片
    1. `array[0:2]`第一行和第二行
    2. `array[,[0:1]]`每行都要，选择第0列和第一列
2. `.reshape((m,n))`:改变数组方式，不改变原来数组形式
3. `.vstack(array1,array2)`:在垂直方向上拼接
4. `.hstack(array1,array2)`:在水平方向上拼接
5. 支持加法和乘法直接操作:
    + 对于形式不相似的数组，也可以进行相应的操作。广播的思想
6. `sum(axis = number)`对相应维度求和
7. `.min()`求最小值
8. `.argmax()`求最大值的索引
9. `.mean()`平均值
10. `.var()`方差
11. `.std()`标准差

## 2.4. array[:,0]写法的理解
1. X[m,n]是通过numpy库引用数组或矩阵中的某一端数据集的一种写法。
2. 通常用法:
    + `X[:,n]`指在全部数组(维)中取第n个数据。
    + `X[m,:]`指取第n集合的所有数据
    + `X[:,m:n]`指取数据集的第m到n-1列数据

## 2.5. 在线性代数中的应用
1. `np.dot()`矩阵内积
2. `linalg.inv(t)`逆矩阵
3. `linalg.det(t)`行列式
4. `linalg.solve(t)`多元一次方程组求根
5. `linalg.eig(t)`求特征值和特征向量

## 2.6. ufunc函数
1. 一种能够对数组的每个元素进行操作的函数，很多这些函数都是在c语言级别上实现的。
2. 数据量大的时候尽量使用NumPy中的通用函数来实现。

# 3. 矩阵的几种乘法

## 3.1. np.multiply()函数
- **数组和矩阵**对应位置相乘
```py
A = np.arange(1,5).reshape(2,2)
B = np.arange(0,4).reshape(2,2)
# 数组场景
C = np.multiply(A,B)
# 矩阵场景
C = np.multiply(np.mat(A),np.mat(B))
```

$$
\begin{bmatrix} 0 & 2 \\ 6 & 12 \\ \end{bmatrix} = \begin{bmatrix} 1 & 2 \\ 3 & 4 \\ \end{bmatrix} multiply \begin{bmatrix} 0 & 1 \\ 2 & 3 \\ \end{bmatrix} 
$$

## 3.2. np.dot()函数
1. 对于秩为1的数组，执行对应位置相乘，然后再相加
2. 对于秩不为1的数组，执行矩阵乘法运算

### 3.2.1. 数组秩不为1
```py
A = np.arange(1,5).reshape(2,2)
B = np.arange(0,4).reshape(2,2)
C = np.dot(A,B)
```

$$
\begin{bmatrix} 4 & 7 \\ 8 & 15 \\ \end{bmatrix} = \begin{bmatrix} 1 & 2 \\ 3 & 4 \\ \end{bmatrix}\ dot\  \begin{bmatrix} 0 & 1 \\ 2 & 3 \\ \end{bmatrix} 
$$

### 3.2.2. 数组秩为1
```py
A = np.arange(1,4)
B = np.arange(0,3)
C = np.dot(A, B)   #对应位置相乘，再求和
```

$$
\begin{bmatrix} 1 & 2 & 3 \end{bmatrix}\ dot\ \begin{bmatrix} 0 & 1 & 2 \end{bmatrix} = 8
$$

### 3.2.3. 矩阵场景
```py
A = np.arange(1,5).reshape(2,2)
B = np.arange(0,4).reshape(2,2)
C = np.dot(np.mat(A), np.mat(B))
```

$$
\begin{bmatrix} 4 & 7 \\ 8 & 15 \\ \end{bmatrix} = \begin{bmatrix} 1 & 2 \\ 3 & 4 \\ \end{bmatrix}\ dot\  \begin{bmatrix} 0 & 1 \\ 2 & 3 \\ \end{bmatrix} 
$$


## 3.3. *乘法运算
1. 数组对应位相乘
2. 矩阵执行矩阵乘法

```py
A = np.arange(1,5).reshape(2,2)
B = np.arange(0,4).reshape(2,2)
C = A*B  #对应位置点乘
D = (np.mat(A))*(np.mat(B)) # 执行矩阵运算
```

# 4. 参考
1. <a href = "https://blog.csdn.net/zenghaitao0128/article/details/78715140">python中np.multiply()、np.dot()和星号(*)三种乘法运算的区别</a>