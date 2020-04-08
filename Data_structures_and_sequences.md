## 3.1  数据结构和序列
### 3.1.1  元组
元组是一种固定长度、不可变的Python对象序列。创建元组最简单的办法就是用逗号分隔序列值。
```python
tup = 4, 5, 6
tup
```
*输出结果：(4, 5, 6)*

当你通过更复杂的表达式来定义元组时，通常需要用括号将值包起来，例如下面这个例子。生成了元素是元组的元组：
```python
nested_tup = (4, 5, 6), (7, 8)
nested_tup
```
*输出结果：((4, 5, 6), (7, 8))*

可以使用tuple函数将任意序列或迭代器转换为元组：
```python
tuple([4, 0, 2])
```
*输出结果：(4, 0, 2)*

```python
tup = tuple('string')
tup
```
*输出结果：('s', 't', 'r', 'i', 'n', 'g')*

元组的元素可以通过中括号[]来获取
```python
tup[0]
```
*输出结果：'s'*

虽然对象元组中存储的对象其自身是可变的，但是元组一旦创建，各个位置上的对象是无法被修改的：
```python
tup = tuple(['foo', [1, 2], True])
tup[2] = False
```
结果如下：
```python
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-15-11b694945ab9> in <module>
      1 tup = tuple(['foo', [1, 2], True])
----> 2 tup[2] = False

TypeError: 'tuple' object does not support item assignment
```

如果元组中的一个对象是可变的，例如列表，你可以在它的内部进行修改:
```python
tup[1].append(3)
tup
```
*输出结果：('foo', [1, 2, 3], True)*

可以使用+号连接元组来生成更长的元组：
```python
# 结尾必须带逗号
(4, None, 'foo') + (6, 0) + ('bar',)
```
*输出结果：(4, None, 'foo', 6, 0, 'bar')*

将元组乘以整数倍，则会和列表一样，生成含有多份拷贝的元组：
```python
('foo', 'bar') * 4
```
*输出结果：('foo', 'bar', 'foo', 'bar', 'foo', 'bar', 'foo', 'bar')*

**请注意对象自身并没有复制，只是指向它们的引用进行了复制。**

#### 3.1.1.1  元组拆包
如果你想要将元组型的表达式赋值给变量，python会对等号右边的值进行拆包：
```python
tup = (4, 5, 6)
a, b, c = tup
b
```
*输出结果：5*

嵌套元素也可以拆包:
```python
tup = 4, 5, (6, 7)
a, b, (c, d) = tup
d
```
*输出结果：7*

在python中，交换可以如下完成：
```python
a, b = 1, 2
a
```
*输出结果：1*

```python
b
```
*输出结果：2*

```python
b, a = a, b
a
```
*输出结果：2*

```python
b
```
*输出结果：1*

拆包的一个常用场景就是遍历元组或列表组成的序列：
```python
seq = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
for a, b, c in seq:
    print('a={0},b={1},c={2}'.format(a, b, c))
```
*输出结果：*
*a=1,b=2,c=3*
*a=4,b=5,c=6*
*a=7,b=8,c=9*

使用特殊的语法*rest，用于在函数调用时获取任意长度位置参数列表
```python
values = 1, 2, 3, 4, 5
a, b, *rest = values
a, b
```
*输出结果：(1, 2)*

```python
rest
```
*输出结果：[3, 4, 5]*

为了方便，很多python编程会使用下划线(_)来表示不想要的变量：
```python
a, b, *_ = values
_
```
*输出结果：[3, 4, 5]*

#### 3.1.1.2  元组方法
