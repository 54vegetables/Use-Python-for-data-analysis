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
由于元组的内容和长度是无法改变的，它的实例方法很少。一个常见的用法是count（列表中也可用），用于计量某个数值在元组中出现的次数：
```python
a = 1, 2, 2, 2, 3, 4, 2
a.count(2)
```
*输出结果：4*

### 3.1.2  列表
与元组不同，列表的长度是可变的，它所包含的内容也是可以修改的。可以使用中括号[]或者list类型函数来定义列表：
```python
a_list = [2, 3, 7, None]
```

```python
tup = 'foo', 'bar', 'baz'
b_list = list(tup)
```
*输出结果：['foo', 'bar', 'baz']*

```python
b_list[1] = 'peekaboo'
b_list
```
*输出结果：['foo', 'peekaboo', 'baz']*

list函数在数据处理中常用于将迭代器或者生成器转化为列表：
```python
gen = range(10)
gen
```
*输出结果：range(0, 10)*

```python
list(gen)  # 将迭代器或者生成器转化为列表
```
*输出结果：[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]*

#### 3.1.2.1  增加和移除元素
使用append方法可以将元素添加到列表的尾部：
```python
b_list.append('dwarf')
b_list
```
*输出结果：['foo', 'peekaboo', 'baz', 'dwarf']*

使用insert方法可以将元素插入到指定的列表位置：
```python
b_list.insert(1, 'red')
b_list
```
*输出结果：['foo', 'red', 'peekaboo', 'baz', 'dwarf']*

插入位置的范围在0到列表长度之间。
**如果你想要在程序的头部和尾部都插入元素，那你应该探索下collections.deque,它是一个双端队列，可以满足头尾部都增加的要求。**

pop()操作会将特定位置的元素移除并返回：
```python
b_list.pop(2)
```
*输出结果：'peekaboo'*

```python
b_list
```
*输出结果：['foo', 'red', 'baz', 'dwarf']*

元素可以通过remove方法移除，该方法会将特定位置的元素移除并返回：
```python
b_list.append('foo')
b_list
```
*输出结果：['foo', 'red', 'baz', 'dwarf', 'foo']*

```python
b_list.remove('foo')
b_list
```
*输出结果：['red', 'baz', 'dwarf']*

使用in关键字可以检查一个值是否在列表中：
```python
'dwarf' in b_list
```
*输出结果：True*

not关键字可以用作in的反义词，表示"不在"：
```python
'dwarf' not in b_list
```
*输出结果：False*

与字典、集合相比，检查列表中包含一个值是非常缓慢的。
这是因为Python在列表中进行了线性逐个扫描，而在字典和集合中Python是同时检查所有元素的（基于哈希表）。

#### 3.1.2.2  连接和联合列表
与元组相似，两个列表可以使用+号连接：
```python
[4, None, 'foo'] + [7, 8, (2, 3)]
```
*输出结果：[4, None, 'foo', 7, 8, (2, 3)]*

如果你有一个已经定义的列表，你可以使用extend方法向该列表添加多个元素：
```python
x = [4, None, 'foo']
x.extend([7, 8, (2, 3)])
x
```
*输出结果：[4, None, 'foo', 7, 8, (2, 3)]*

使用extend将元素添加到已经存在的列表是更好的方式，尤其是在你需要构建一个大型列表时：
```python
everything = []
for chunk in list_of_lists:
    everything.extend(chunk)
```

上述实现比下述实现更快：
```python
everything = []
for chunk in list_of_lists:
    everything += chunk
```

#### 3.1.2.3  排序
你可以调用列表的sort方法对列表进行内部排序（无须新建一个对象）：
```python
a = [7, 2, 5, 1, 3]
a.sort()
a
```
*输出结果：[1, 2, 3, 5, 7]*

传递一个二级排序key---一个用于生成排序值的函数。
```python
b = ['saw', 'small', 'He', 'foxes', 'six']
b.sort(key=len)
b
```
*输出结果：['He', 'saw', 'six', 'small', 'foxes']*

