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

#### 3.1.2.4  二分搜索和已排序列表的维护
内建的bisect模块实现了二分搜索和已排序列表的插值。
bisect.bisect会找到元素应当被插入的位置，并保持序列排序。
```python
import bisect
c = [1, 2, 2, 2, 3, 4, 7]
bisect.bisect(c, 2)
```
*输出结果：4*

bisect.insort将元素插入到相应位置
```python
bisect.insort(c, 6)
c
```
*输出结果：[1, 2, 2, 2, 3, 4, 6, 7]*

bisect模块的函数并不会检查列表是否已经排序。对未排序列表使用disect的函数虽然不会报错，但可能会导致不正确的结果。

#### 3.1.2.5  切片
使用切片符号可以对大多数序列类型选取其子集，它的基本形式是将start:stop传入到索引符号[]中:
```python
seq = [7, 2, 3, 7, 5, 6, 0, 1]
seq[1 : 5]
```
*输出结果：[2, 3, 7, 5]*

切片还可以将序列赋值给变量：
```python
seq[3 : 4] = [6, 3]
seq
```
*输出结果：[7, 2, 3, 6, 3, 5, 6, 0, 1]*

**由于起始位置start的索引是包含的，而结束位置stop的索引并不包含，因此元素的数量是stop-start。**

start和stop是可以省略的。如果省略的话会默认传入序列的起始位置或结束位置：
```python
seq[:5]
```
*输出结果：[7, 2, 3, 6, 3]*

```python
seq[3:]
```
*输出结果：[6, 3, 5, 6, 0, 1]*

负索引可以从序列的尾部进行索引：
```python
seq[-4:]
```
*输出结果：[5, 6, 0, 1]*

```python
seq[-6:-2]
```
*输出结果：[6, 3, 5, 6]*

步进值steo可以在第二个冒号后面使用，意思是每隔多少个数取一个值：
```python
seq[::2]
```
*输出结果：[7, 3, 3, 6, 1]*

当需要对列表或元组进行翻转时，可以将步进值设置为-1
```python
seq[::-1]
```
*输出结果：[1, 0, 6, 5, 3, 6, 3, 2, 7]*

### 3.1.3  内建序列函数
#### 3.1.3.1  enumerate
在遍历一个序列的同时追踪当前元素的索引<br>
一种自行实现的方法像下面的示例：
```python
i = 0
for value in collection:
    # 使用值做点事
    i += 1
```
Python内建了enumerate函数，返回了(i, value)元组的序列，其中value是元素的值，i是元素的索引：
```python
for i, value in enumerate(collection):
    # 使用值做点事
```

使用enumerate构造一个字典，将序列值(假设是唯一的)映射到索引位置上：
```python
some_list = ['foo', 'bar', 'baz']
mapping = {}
for i, v in enumerate(some_list):
    mapping[i] = v
mapping
```
*输出结果：0: 'foo', 1: 'bar', 2: 'baz'}*

#### 3.1.3.2  sorted
sorted函数返回一个根据任意序列中的元素新建的已排序列表：
```python
sorted([7, 1, 2, 6, 0, 3, 2])
```
*输出结果：[0, 1, 2, 2, 3, 6, 7]*

```python
sorted('horse race')
```
*输出结果：[' ', 'a', 'c', 'e', 'e', 'h', 'o', 'r', 'r', 's']*

#### 3.1.3.3  zip
zip将列表、元组或其他序列的元素配对，新建一个元组构成的列表：
```python
seq1 = ['foo', 'bar', 'baz']
seq2 = ['one', 'two', 'three']
zipped = zip(seq1, seq2)  # 将seq1和seq2纵向压缩组成元组存到列表中
# 打印解压后的值
print(*zipped)
```
*输出结果：('foo', 'one') ('bar', 'two') ('baz', 'three')*

```python
seq1 = ['foo', 'bar', 'baz']
seq2 = ['one', 'two', 'three']
zipped = zip(seq1, seq2)  # 将seq1和seq2纵向压缩组成元组
# 拆包将解压后的元素x、y、z提取出来
x, y, z = [*zipped]
x
```
*输出结果：('foo', 'one')*

```python
y
```
*输出结果：('bar', 'two')*

```python
z
```
*输出结果：('baz', 'three')*

zip可以处理任意长度的序列，它生成列表长度由最短的序列决定：
```python
seq3 = [True, False]
zipped = zip(seq1, seq2, seq3)
list(zipped)
```
*输出结果：[('foo', 'one', True), ('bar', 'two', False)]*

zip的常用场景为同时遍历多个序列，有时候会和enumerate同时使用：
```python
for i, (a, b) in enumerate(zip(seq1, seq2)):
    print('{0}: {1}, {2}'.format(i, a, b))
```
*输出结果如下：*
*0: foo, one
1: bar, two
2: baz, three*

使用zip函数将行的列表转换为列的列表：
```python
pitchers = [('Nolan', 'Ryan'), ('Roger', 'Clemens'), ('Schilling', 'Curt')]
first_names, last_names = zip(*pitchers)
first_names
```
*输出结果：('Nolan', 'Roger', 'Schilling')*

```python
last_names
```
*输出结果：('Ryan', 'Clemens', 'Curt')*

#### 3.1.3.4  reversed
reversed函数将序列的元素倒序排列：
```python
reversed([range(10)])
```
*输出结果：<list_reverseiterator at 0x18f297f8b08>*

```python
list(reversed([range(10)]))
```
*输出结果：[range(0, 10)]*

**reversed是一个生成器，如果没有实例化（例如使用list函数或进行for循环）的时候，它并不会产生一个倒序列表。**

### 3.1.4  字典
dict（字典）更为常用的名字是哈希表或者是关联数组。字典是拥有灵活尺寸的键值对集合，其中键和值都是Python对象。<br>
**用大括号{}是创建字典的一种方式，在字典中用逗号将键值对分隔。**

```python
empty_dict = {}
d1 = {'a': 'some value', 'b': [1, 2, 3, 4]}
d1
```
*输出结果：{'a': 'some value', 'b': [1, 2, 3, 4]}*

你可以访问、插入或设置字典中的元素，就像访问列表和元组的元素一样：
```python
d1[7] = 'an integer'
d1
```
*输出结果：{'a': 'some value', 'b': [1, 2, 3, 4], 7: 'an integer'}*

```python
d1['b']
```
*输出结果：[1, 2, 3, 4]*

你可以用检查列表或元组中是否含有一个元素的相同语法来检查字典是否有一个键：
```python
'b' in d1
```
*输出结果：True*

你可以使用del关键字或pop方法删除值，pop方法会在删除的同时返回被删除的值,并删除键：
```python
d1[5] = 'some value'
d1
```
*输出结果：{'a': 'some value', 'b': [1, 2, 3, 4], 7: 'an integer', 5: 'some value'}*

```python
d1['dummy'] = 'another value'
d1
```
*输出结果如下：*
*{'a': 'some value',
'b': [1, 2, 3, 4],
7: 'an integer',
5: 'some value',
'dummy': 'another value'}*
 
使用del 字典名[key]删除字典中的键值对
```python
del d1[5]
d1
```
*输出结果如下：*
*{'a': 'some value',
 'b': [1, 2, 3, 4],
 7: 'an integer',
 'dummy': 'another value'}*

使用字典名.pop(key) 删除字典中的键值对
```python
ret = d1.pop('dummy')
ret
```
*输出结果：'another value'*

```python
d1
```
*输出结果：{'a': 'some value', 'b': [1, 2, 3, 4], 7: 'an integer'}*

keys方法和values方法会分别为你提供字典键、值得迭代器。然而键值对并没有特定的顺序，这些函数输出的键、值都是按照相同的顺序：
```python
list(d1.keys())
```
*输出结果：['a', 'b', 7]*
 
```python
list(d1.values())
```
*输出结果：['some value', [1, 2, 3, 4], 'an integer']*
 
你可以使用update方法将两个字典合并：
```python
d1.update({'b' : 'foo', 'c' : 12})
d1
```
*输出结果：{'a': 'some value', 'b': 'foo', 7: 'an integer', 'c': 12}*

**对于任何原字典中已经存在的键，如果传给update方法的数据也会含有相同的键，则它的值将会被覆盖。**

#### 3.1.4.1  从序列生成字典
通常情况下，你会有两个序列想要在字典中按元素配对。起初，你可能会写出这样的代码：
```python
mapping = {}
# 纵向压缩，建立key_list和key_value一一对应的关系，再进行拆包，提取出key和value
for key, value in zip(key_list, key_value): 
    # 将key和value形成键值对，添加到字典中    
    mapping[key] = value
```

由于字典本质上是一个2-元组(含有2个元素的元组)的集合，字典是可以接受一个2-元组的列表作为参数的：
```python
# 将0-4的数组和4-0的数组纵向压缩，使用dict函数存放到字典中
mapping = dict(zip(range(5), reversed(range(5))))  
mapping
```
*输出结果：{0: 4, 1: 3, 2: 2, 3: 1, 4: 0}*

#### 3.1.4.2 默认值
通常情况下，会有这样的代码逻辑：
```python
if key in some_list:
    value = some_list[key]
else:
    value = defalut_value
```
字典的get方法和pop方法可以返回一个默认值，上述的if-else代码块可以被简写为：
```python
value = some_dict.get(key, defalut_value)
```
**带有默认值的get方法会在key参数不是字典的键时返回None，而pop会抛出异常。**

some_dict.setdefault(key, default=None)<br>
key:要查的key值<br>
defalut:如果不在添加key值并默认value值
```python
dict_1 = {0: 'a', 1: 'b', 2: 'c'}
dict_1.setdefault(3, None)
dict_1
```
*输出结果：{0: 'a', 1: 'b', 2: 'c', 3: None}*

将字词组成的列表根据首字母分类为包含列表的字典：
```python
# method1
words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter = {}
for word in words:
    letter = word[0]
    if letter in by_letter:
        by_letter[letter].append(word)
    else:
        by_letter[letter] = [word]
by_letter
```
*输出结果：{'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}*

```python
# method2
# some_dict.setdefault(key, []).append(word)
# key:你要查的key值
# []:value值的类型
words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter = {}
for word in words:
    letter = word[0]
    by_letter.setdefault(letter, []).append(word)
    # 字典名.setdefault(首字母, 默认列表类型).append(单词)   
by_letter
```
*输出结果：{'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}*

内建的集合模块有一个非常有用的类---defaultdict。想要生成符合要求的字典，你可以向字典中传入类型或能在各位置生成默认值的函数：
```python
# method3
from collections import defaultdict
words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter = defaultdict(list)  # 实例化一个默认空字典，值类型为list型
for word in words:
    # 如果首字母在by_letter的键里，就在对应的值列表中添加word；如果不在，则在空列表中添加word
    by_letter[word[0]].append(word)  
dict(by_letter)
```
*输出结果：{'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}*

#### 3.1.4.3  有效的字典键类型
Python字典的值可以是任何Python对象，但键必须是不可变的对象。
**不可变对象---可以哈希化：标量（int、float、bool、str、tuple）**
**可变对象---不可以哈希化：list、dict、set**

```python
hash('string')
```
*输出结果：5621000962522254165*

```python
hash((1, 2, (2, 3)))
```
*输出结果：1097636502276347782*

```python
hash((1, 2, [3, 4]))
```
*输出结果如下：*

```python
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-15-1d101f826a35> in <module>
----> 1 hash((1, 2, [3, 4]))

TypeError: unhashable type: 'list'
```

为了将列表作为键，一种方式就是将其转换为元组，而元组只要它内部元素都可以哈希化，则它自己也可以哈希化：
```python
d = {}
d[tuple([1, 2, 3])] = 5  # tuple函数将列表转换为元组
d
```
*输出结果:{(1, 2, 3): 5}*

### 3.1.5  集合
集合是一种无序且元素唯一的容器。集合像字典一样，但它有键没有值。<br>
集合可以有两种创建方式：1)set函数   2){字面值集}
```python
# 1)set函数
set([2, 2, 2, 1, 3, 3])   # set函数将列表转换为集合,重复元素去掉
```
*输出结果:{1, 2, 3}*

```python
# 2){字面值集}
{2, 2, 2, 1, 3, 3}
```
*输出结果:{1, 2, 3}*

集合运算：<br>
1.联合：两个集合不同元素的并集。（union方法或|二元操作符）
```python
a = {1, 2, 3, 4, 5}
b = {3, 4, 5, 6, 7, 8}
a.union(b)  # a集合本身没变
```
*输出结果:{1, 2, 3, 4, 5, 6, 7, 8}*

```python
a | b
```
*输出结果:{1, 2, 3, 4, 5, 6, 7, 8}*

2.将a的内容设置为a和b的并集。(update方法或|=二元操作符)
```python
c = a.copy()
c.update(b)   # c集合发生变化
c
```
*输出结果:{1, 2, 3, 4, 5, 6, 7, 8}*

```python
c = a.copy()
c |= b       # c集合发生变化
c
```
*输出结果:{1, 2, 3, 4, 5, 6, 7, 8}*

3. 交集：a、b中同时包含的元素。(intersection方法和&二元操作符)
```python
a.intersection(b)  # a本身不发生变化
```
*输出结果:{3, 4, 5}*

```python
a & b
```
*输出结果:{3, 4, 5}*

4. 将a的内容设置为a和b的交集(intersection_update方法和&=二元操作符)
```python
d = a.copy()
d.intersection_update(b)  # d的内容发生变化
d
```
*输出结果:{3, 4, 5}*

```python
d = a.copy()
d &= b
d
```
*输出结果:{3, 4, 5}* 

5. 在a不在b的元素(difference方法和-二元操作符)
```python
a.difference(b)
```
*输出结果:{1, 2}*

```python
a - b
```
*输出结果:{1, 2}*

6. 将a的内容设为在a不在b的元素。(difference_update方法和-=二元操作符)
```python
e = a.copy()
e.difference_update(b)  # e的内容发生变化
e
```
*输出结果:{1, 2}*

```python
e = a.copy()
e -= b
e
```
*输出结果:{1, 2}*

7.所有在a或b中，但不是同时在a、b中的元素。(symmetric_difference方法和^二元操作符)
```python
a.symmetric_difference(b)  # a本身不发生变化
```
*输出结果:{1, 2, 6, 7, 8}*

```python
a ^ b
```
*输出结果:{1, 2, 6, 7, 8}*

8. 将a的内容设为所有在a或b中，但不是同时在a、b中的元素。(symmetric_difference_update方法和^=二元操作符)
```python
f = a.copy()
f.symmetric_difference_update(b)  # f的内容发生变化
f
```
*输出结果:{1, 2, 6, 7, 8}*

```python
f = a.copy()
f ^= b 
f
```
*输出结果:{1, 2, 6, 7, 8}*

9.检查一个集合是否是另一个集合的子集(包含于)。(issubset方法)
```python
a_set = {1, 2, 3, 4, 5,}
{1, 2, 3}.issubset(a_set)
```
*输出结果:True*

10.检查一个集合是否是另一个集合的超集（包含）。(issuperset方法)
```python
a_set.issuperset({1, 2, 3})
```
*输出结果:True*

当且仅当两个集合的内容一模一样时，两个集合才相等：
```python
{1, 2, 3} == {1, 2, 3}
```
*输出结果:True*

11.a、b没有交集返回True。(isdisjoint方法)
```python
a_set = {1, 3, 5, 7, 9}
b_set = {2, 4, 6, 8, 10}
a_set.isdisjoint(b_set)
```
*输出结果:True*

和字典类似，集合的元素必须是不可变的。如果想要包含列表型的元素，必须先转换为元组：
```python
my_data = [1, 2, 3, 4]
my_set = {tuple(my_data)}
my_set
```
*输出结果:{(1, 2, 3, 4)}*

### 3.1.6  列表、集合和字典的推导式
列表推导式允许你过滤一个容器的元素，用一种简明的表达式转换给过滤器的元素，从而生成一个新的列表。
```python
# 列表解析
[expr for val in collection if condition]

# 这与下面的for循环是等价的：
result = []
for val in collection:
    if condition:
        result.append(expr)
```

给定一个字符串列表，可以过滤出长度大于2的，并且将字母改为大写：
```python
strings = ['a', 'as', 'bat', 'car', 'dove', 'python']
[x.upper() for x in strings if len(x) > 2]
```
*输出结果:['BAT', 'CAR', 'DOVE', 'PYTHON']*

字典推导式
```python
dict_comp = {key-expr : value-expr for value in collection if condition}
```
编写代码：创建一个字符串与其位置匹配的字典作为字典推导式的简单示例
```python
loc_mapping = {value : index for index, value in enumerate(strings)}
loc_mapping
```
*输出结果:{'a': 0, 'as': 1, 'bat': 2, 'car': 3, 'dove': 4, 'python': 5}*

集合推导式
```python
set_comp = {expr for value in collection if condition}
```
编写代码：集合里包含列表中字符串的长度
```python
unique_lengths = {len(x) for x in strings}
unique_lengths
```
*输出结果:{1, 2, 3, 4, 6}*

使用map函数更函数化、更简洁地表达：
```python
set(map(len, strings))
```
*输出结果:{1, 2, 3, 4, 6}*

#### 3.1.6.1  嵌套列表推导式
有一个包含列表的列表，内容是英语姓名和西班牙语姓名
```python
all_data = [['John', 'Emily', 'Michael', 'Mary', 'Steven'], ['Maria', 'Juan', 'Javier', 'Natalia', 'Pilar']]
```
想要获得一个列表所有含有2个以上字母e的名字
```python
# method1
names_of_interest = []
for names in all_data:
    for name in names:
        if name.count('e') >= 2:
            names_of_interest.append(name)
names_of_interest
```
*输出结果:['Steven']*

```python
# method2
names_of_interest = []
for names in all_data:
    enough_es = [name for name in names if name.count('e') >= 2]
    names_of_interest.extend(enough_es)
names_of_interest
```
*输出结果:['Steven']*

```python
# method3:嵌套列表推导式
names_of_interest = []
result = [name for names in all_data for name in names if name.count('e') >= 2]
result
```
*输出结果:['Steven']*

**列表推导式的for循环部分是根据嵌套的顺序排列的，所有的过滤条件像之前一样被放在尾部。**

将含有整数元组的列表扁平化为一个简单的整数列表：
```python
some_tuples = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
# method1
flattened = []
for tuple in some_tuples:
    for x in tuple:
        flattened.append(x)
flattened
```
*输出结果:[1, 2, 3, 4, 5, 6, 7, 8, 9]*

```python
# method2
flattened = [x for tuple in some_tuples for x in tuple]
flattened
```
*输出结果:[1, 2, 3, 4, 5, 6, 7, 8, 9]*

列表推导式中的列表推导式
```python
[[x for x in tuple] for tuple in some_tuples]
```
*输出结果:[[1, 2, 3], [4, 5, 6], [7, 8, 9]]*
