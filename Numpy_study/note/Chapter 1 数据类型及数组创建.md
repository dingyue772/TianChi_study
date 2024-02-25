# 常量
Numpy中的常量包括numpy.nan、numpy.inf、numpy.pi、numpy.e
**numpy.nan**
numpy.nan表示空值，nan=NaN=NAN
**numpy.inf**
正无穷大
Inf=inf=infty=Infinity=PINF
**numpy.pi**
圆周率Π
pi= 3.1415926535897932384626433...
**numpy.e**
自然常数
e = 2.7182818284590452353602874713526624977572...
# 数据类型
**常见数据类型**
bool、int、float、str等python原生数据类型在numpy中的使用时，末尾都加上了`_`
![image.png](https://piclist-1317108127.cos.ap-nanjing.myqcloud.com/%E5%A4%A7%E4%B8%89%E4%B8%8A/20240225152537.png)

**创建数据类型**
使用`dtype`类对象进行创建
```python
class dtype(object):
    def __init__(self, obj, align=False, copy=False):
        pass
```

每个内建数据类型和其对应的字符代码表
![image.png](https://piclist-1317108127.cos.ap-nanjing.myqcloud.com/%E5%A4%A7%E4%B8%89%E4%B8%8A/20240225152640.png)

**数据类型信息**
`a.max` `a.min`
python和numpy中`int`类型的区别：
在Python中，整数类型（`int`）是动态的，只要内存允许，Python会自动扩展整数的大小，而不会抛出溢出错误。这种使得Python在处理大整数时非常方便，但也可能在某些情况下导致意外的结果，因为整数的大小不再是固定的。
NumPy是基于C语言实现的，遵循C语言的数据类型规则。NumPy中的整数类型（如`numpy.int32`）有其固定的表示范围，超出这个范围的运算会导致溢出（overflow），返回一个负数或者其他不可预期的值。
# 时间日期和时间增量
**datetime64基础**
`datetime64`是带单位的日期时间类型
`datetime64`中的日期时间单位和对应的含义
![image.png](https://piclist-1317108127.cos.ap-nanjing.myqcloud.com/%E5%A4%A7%E4%B8%89%E4%B8%8A/20240225152820.png)

**datetime64和timedelta64运算**
`timedelta64` 表示两个 `datetime64` 之间的差。`timedelta64` 也是带单位的，并且和相减运算中的两个 `datetime64` 中的较小的单位保持一致。
注意：
年（'Y'）和月（'M'）这两个单位无法和其它单位进行运算（一年有几天？一个月有几个小时？这些都是不确定的）
**datetime64的应用**
numpy包含一组与工作日（busday）有关的方法
`numpy.busday_offsets` 处理与工作日相关的日期偏移计算
```python
numpy.busday_offset(dates, offsets, roll='raise', weekmask='1111100', holidays=None, busdaycal=None, out=None)
# 函数说明：dates表示要处理的datetime64日期数组；offsets类似于整数的数组。这是要应用于每个日期的偏移量；roll用于指定如何处理不在工作日的日期
```
`numpy.is_busday` 计算指定日期是否为工作日
```python
numpy.is_busday(dates, weekmask='1111100', holidays=None, busdaycal=None, out=None)
```
`numpy.busday_count` 计算两个日期之间的工作日数量
```python
numpy.busday_count(begindates, enddates, weekmask='1111100', holidays=[], busdaycal=None, out=None)
```
# 数组的创建
即得到numpy数据结构`ndarray`
## 依据现有数据创建ndarray
**1. array()函数**
```python
def array(p_object, dtype=None, copy=True, order='K', subok=False, ndmin=0):
```
**2. asarray()函数**
对比`array()`函数和`asarray()`函数：
当数据源为`ndarray`时，`array()`仍然会 copy 出一个副本，占用新的内存，但 `asarray()`不会，而是与原来的`ndarray`共享存储空间
```python
def asarray(a, dtype=None, order=None):
    return array(a, dtype, copy=False, order=order)
```
**3. fromfunction()函数**
```python
def fromfunction(function, shape, **kwargs):
```
通过在每个坐标上执行一个函数来构造数组
## 依据ones和zeros填充方式
> 适用场景：机器学习任务中的初始化参数，需要一个固定大小的常数值/随机值矩阵

**零数组**
`zeros()`函数：返回给定形状和类型的零数组
`zeros_like()`函数：返回与给定数组形状和类型相同的零数组
```python
def zeros(shape, dtype=None, order='C'):
def zeros_like(a, dtype=None, order='K', subok=True, shape=None):
```

**1数组**
`ones()`函数：返回给定形状和类型的1数组
`ones_like()`函数：返回与给定数组形状和类型相同的1数组
```python
def ones(shape, dtype=None, order='C'):
def ones_like(a, dtype=None, order='K', subok=True, shape=None):
```

**空数组**
`empty()`函数：返回一个空数组，数组元素为随机数
`empty_like()`函数：返回与给定数组具有相同形状和类型的新数组
```python
def empty(shape, dtype=None, order='C'): 
def empty_like(prototype, dtype=None, order='K', subok=True, shape=None):
```

**单位数组**
`eye()`函数：返回一个对角线上为1，其它地方为零的单位数组
`identity()`函数：返回一个方的单位数组
```python
def eye(N, M=None, k=0, dtype=float, order='C'):
def identity(n, dtype=None):
```

**对角数组**
`diag()`函数：提取对角线或构造对角数组
```python
def diag(v, k=0):
```

**常数数组**
`full()`函数：返回一个常数数组
`full_like()`函数：返回与给定数组具有相同形状和类型的常数数组
```python
def full(shape, fill_value, dtype=None, order='C'):
def full_like(a, fill_value, dtype=None, order='K', subok=True, shape=None):
```

## 利用数值范围来创建ndarray
`arange()`函数：返回给定间隔内的均匀间隔的值
`linspace()`函数：返回指定间隔内的等间隔数字
`logspace()`函数：返回数以对数刻度均匀分布
`numpy.random.random()`函数：返回一个由`[0,1)`内的随机数组成的数组
```python
def arange([start,] stop[, step,], dtype=None): 
def linspace(start, stop, num=50, endpoint=True, retstep=False, 
             dtype=None, axis=0):
def logspace(start, stop, num=50, endpoint=True, base=10.0, 
             dtype=None, axis=0):
def random(d0, d1, ..., dn): 
```
## 结构数组创建
将`np.array()`创建数组时的参数`dtype`修改为自定义的结构，举例说明如下
**1. 利用字典创建结构**
```python
personType = np.dtype({
    'names': ['name', 'age', 'weight'],
    'formats': ['U30', 'i8', 'f8']})
```

**2. 利用包含多个元组的列表来定义结构**
```python
personType = np.dtype([('name', 'U30'), ('age', 'i8'), ('weight', 'f8')])
```
# 数组的属性
`numpy.ndarray.ndim`：返回**数组维数**（轴的个数）也称为秩，一维数组的秩为 1，二维数组的秩为 2
`numpy.ndarray.shape`表示**数组维度**，返回一个元组，这个元组的长度就是维度的数目，即 `ndim` 属性(秩)
`numpy.ndarray.size`数组中**元素个数**，相当于数组的`shape`中所有元素的乘积，例如矩阵的元素总量为行与列的乘积
`numpy.ndarray.dtype` `ndarray` 对象的**元素类型**
`numpy.ndarray.itemsize`返回数组中**每一个元素的字节大小**

```python
class ndarray(object):
    shape = property(lambda self: object(), lambda self, v: None, lambda self: None)
    dtype = property(lambda self: object(), lambda self, v: None, lambda self: None)
    size = property(lambda self: object(), lambda self, v: None, lambda self: None)
    ndim = property(lambda self: object(), lambda self, v: None, lambda self: None)
    itemsize = property(lambda self: object(), lambda self, v: None, lambda self: None)
```

**注意**
在`ndarray`中所有元素必须是同一类型，否则会自动向下转换，`int->float->str`
