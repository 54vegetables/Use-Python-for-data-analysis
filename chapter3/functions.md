## 3.2  函数
函数是Python中最重要、最基础的代码组织和代码复用方式。根据经验，如果你需要多次重复相同或类似的代码，就非常值得写一个可复用的函数。
<br>
函数声明时使用def关键字，返回时使用return关键字：
```python
def my_function(x, y, z=1.5):   # x和y是位置参数，z是关键字参数
    if z > 1:
        return z * (x + y)
    else:
        return z / (x + y)
```
**如果Python达到函数的尾部时仍然没有遇到return语句，就会自动返回None。**

每个函数都可以有位置参数和关键字参数。关键字参数最常用于指定默认值或可选参数。<br>
函数可以通过以下任意一种方式进行调用：
```python
my_function(5, 6, z=0.7)
```
*输出结果：0.06363636363636363*

```python
my_function(3.14, 7, 3.5)
```
*输出结果：35.49*

```python
my_function(10, 20)
```
*输出结果：45.0*

**函数参数的主要限制是关键字参数（函数的默认值）必须跟在位置参数后。**

也可以使用关键字参数向位置参数传参，可以按照任意顺序指定关键字参数。
```python
my_function(x=5, y=6, z=7)
```
*输出结果：77*

```python
my_function(y=6, x=5, z=7)
```
*输出结果：77*

### 3.2.1  命名空间、作用域和本地函数
函数有两种连接变量的方式：全局、本地。描述变量作用域的名称是命名空间。<br>
在函数内部，任意变量都是默认分配到本地命名空间的。<br>
本地命名空间是在函数被调用时生成的，并立即由函数的参数填充。<br>
当函数执行结束后，本地命名空间就会被销毁（除了一些特殊情况）<br>
```python
# 1.在内部定义，外部不定义
def func():
    a = []
    for i in range(5):
        a.append(i)
    print(a)
func()
```
*输出结果：[0, 1, 2, 3, 4]*

当调用func()时，空的列表a会创建，5个元素被添加到列表，之后a会在函数退出时被销毁。
```python
def func():
    a1 = []
    for i in range(5):
        a1.append(i)
func()
print(a1)
```
*输出结果如下：*

```python
NameError                                 Traceback (most recent call last)
<ipython-input-16-8c2348bb4c00> in <module>
      4         a1.append(i)
      5 func()
----> 6 print(a1)

NameError: name 'a1' is not defined
```

```python
# 2.外部定义，内部不定义
a_2 = []
def func():
    for i in range(5):
        a_2.append(i)
#     print(a_2)
#     print(id(a_2))
func()
print(a_2)
print(id(a_2))
```
*输出结果如下：*
*[0, 1, 2, 3, 4]
2132411855112*

```python
# 3.内部定义和外部定义
a_3 = []
def func():
    a_3 = []
    for i in range(5):
        a_3.append(i)
#     print(a_3)
#     print(id(a_3))
func()
print(a_3)
print(id(a_3))
```
*输出结果如下：*
*[]
1562447033032*

4.在函数内部使用使用并更改全局变量，函数内部的变量必须使用global关键字声明为全局变量:
```python
a = None
def blind_a_variable():
    global a
    a = []
blind_a_variable()
print(a)
```
*输出结果：[]*

### 3.2.2  返回多个值
1.返回元组对象
```python
def f():
    a = 5
    b = 6
    c = 7
    return a, b, c
# 返回一个元组对象，将元组拆包
a, b, c = f()
print('a={0}, b={1}, c={2}'.format(a, b, c))

# 返回一个元组对象
result = f()
result
```
*输出结果如下：*
*a=5, b=6, c=7
(5, 6, 7)*

2.返回字典对象
```python
def f():
    a = 5
    b = 6
    c = 7
    return {'a' : a, 'b' : b, 'c' : c}
result = f()
# values()函数返回字典值的迭代器，加上list才会以列表形式输出
list(result.values())
```
*输出结果：[5, 6, 7]*

### 3.2.3  函数是对象
假设我们正在做数据清洗，需要将一些变形应用到下列字符串列表中：
```python
states = ['Alabama ', 'Georgia!', 'Georgia', 'georgia', 'FlOrIda', 'south carolina##', 'West virginia?']
```
我们需要：去空格，移除标点符号，调整适当的大小写<br>
方法一：使用内建的字符串方法，结合标准库中的正则表达式模块re
```python
import re

states = ['  Alabama ', 'Georgia!', 'Georgia', 'georgia', 'FlOrIda', 'south   carolina##', 'West virginia?']

def clean_strings(strings):
    result = []
    for value in strings:
        value = value.strip()               # 去掉两端空格
        value = re.sub('[!#?]', '', value)  # 去掉特殊字符!#?
        value = re.sub(' +', ' ', value)  # 将1个或一个以上的空格替换成一个空格
        value = value.title()               # 首字母大写
        result.append(value)
    return result

clean_strings(states)
```
*输出结果如下：*
*['Alabama',
 'Georgia',
 'Georgia',
 'Georgia',
 'Florida',
 'South Carolina',
 'West Virginia']*
 
 方法二：将特定的列表操作应用到某个字符串的集合上
 ```python
 import re

def remove_punctuation(value):
    # 去掉特殊字符#?!
    return re.sub('[#?!]', '',value)

def remove_blank_space(value):
    # 去掉单词之间多余空格
    return re.sub(' +', ' ', value)

def clean_strings(strings, ops):
    result = []
    for value in strings:    # 遍历列表中的每个字符串
        for function in ops: # 遍历函数列表的每个函数
            value = function(value)
        result.append(value)
    return result

states = ['  Alabama ', 'Georgia!', 'Georgia', 'georgia', 'FlOrIda', 'south   carolina##', 'West virginia?']
# 函数操作列表
clean_ops = [str.strip, remove_punctuation, remove_blank_space, str.title]
clean_strings(states, clean_ops)
```
*输出结果如下：*
*['Alabama',
 'Georgia',
 'Georgia',
 'Georgia',
 'Florida',
 'South Carolina',
 'West Virginia']*
 
 方法三：map(函数, 列表)：将一个函数应用到一个序列上
 ```python
  result_1 ,result_2 ,result_3 ,result_4= [],[],[],[]
for x in map(str.strip, states):
    # 去除两端多余空格
    result_1.append(x)
for x in map(remove_punctuation, result_1):
    # 去除特殊字符！#？
    result_2.append(x)
for x in map(remove_blank_space, result_2):
    # 将单词间一个以上空格改为一个空格
    result_3.append(x)
for x in map(str.title, result_3):
    # 字符串首字母大写
    result_4.append(x)
for x in result_4:
    print(x)
```
*输出结果如下：*
*Alabama
Georgia
Georgia
Georgia
Florida
South Carolina
West Virginia*

### 3.2.4 匿名（Lambda）函数
匿名函数是一种通过单个语句生成函数的方式，其结果是返回值。
匿名函数使用lambda关键字定义，该关键字仅表达"我们声明一个匿名函数"的意思：
```python
def short_function(x):
    return x * 2

equiv_anon = lambda x : x * 2
```
匿名函数代码量小(也更为清晰)，将它作为参数进行传值，比写一个完整的函数或者将匿名函数赋值给局部变量更好。
```python
# 1）将列表中的每个值乘以2后输出
def apply_to_list(some_list, f):
    return [f(x) for x in some_list]

ints = [4, 0, 1, 5, 6]
apply_to_list(ints, lambda x: x*2)
```
*输出结果:[8, 0, 2, 10, 12]*

```python
# 2）根据字符串中不同字母的数量对一个字符串集合进行排序：
strings = ['foo', 'card', 'bar', 'aaaa', 'abab']

# list(x)：将每个在列表中的元素拆分成单个小列表
# set函数：去掉单个单词中的元素，转换成集合
# len函数：计算集合的长度
# 最后按照元素个数从小到大对字符列表进行排序
strings.sort(key = lambda x: len(set(list(x))))
strings
```
*输出结果:['aaaa', 'foo', 'abab', 'bar', 'card']*

```python
[list(x) for x in strings]
```
*输出结果如下：*
*[['a', 'a', 'a', 'a'],
 ['f', 'o', 'o'],
 ['a', 'b', 'a', 'b'],
 ['b', 'a', 'r'],
 ['c', 'a', 'r', 'd']]*

```python
[set(list(x)) for x in strings]
```
*输出结果：[{'a'}, {'f', 'o'}, {'a', 'b'}, {'a', 'b', 'r'}, {'a', 'c', 'd', 'r'}]*

**lambda函数被称为匿名函数的原因：和def关键字声明的函数不同，匿名函数对象自身并没有一个显示的_name_属性。**

### 3.2.5 柯里化：部分参数应用
柯里化是计算机科学术语(以数学家Haskell Curry命名)，他表示通过部分参数应用的方式从已有的函数中衍生出新的函数。
<br>
方法一：使用匿名函数
```python
def add_numbers(x, y):
    return x + y

# 定义一个新函数，新函数调用了已经存在的函数
add_five = lambda y: add_numbers(5, y)
add_five(8)
```
*输出结果：13*

方法二：使用functools模块中的pratial函数
```python
from functools import partial

add_five = partial(add_numbers, y=5)  # partial(已定义的函数,改变的值)
add_five(2)
```
*输出结果：7*

### 3.2.6  生成器
通过一致的方式遍历序列，例如列表中的对象或者文件中的一行行内容，这是Python的一个重要特性。<br>
这个特性是通过迭代器协议来实现的，迭代器协议是一种令对象可遍历的通用方式。<br>
例如，遍历一个字典，获得字典的键：
```python
some_dict = {'a': 1, 'b': 2, 'c': 3}
for key in some_dict:
    print(key)
```
*输出结果如下：*
*a
b
c*

当你写下for key in some_dict的语句时，Python解释器首先尝试根据some_dict生成一个迭代器：
```python
dict_iterator = iter(some_dict)
dict_iterator
```
*输出结果：<dict_keyiterator at 0x22eca14acc8>*

大部分以列表或列表型对象为参数的方法都可以接受任意的迭代器对象。<br>
包括内建方法比如min、max和sum，以及类型构造函数比如list和tuple：<br>
```python
list(dict_iterator)
```
*输出结果：['a', 'b', 'c']*

1.生成器是构造新的可遍历对象的一种非常简洁的方式。<br>
2.普通函数执行并一次返回单个结果，而生成器则"惰性"地返回一个多结果序列，在每一个元素产生之后暂停，直到下一个请求。<br>
3.如需创建一个生成器，只需要在函数中将返回关键字return替换为yield关键字：
```python
def squares(n=10):
    print('Generating squares from 1 to {0}'.format(n ** 2))
    for i in range(n + 1):
        yield i ** 2
```

当你实际调用生成器时，代码并不会立即执行：
```python
gen = squares()
gen
```
*输出结果：<generator object squares at 0x0000022EC8B93C48>*

直到你请求生成器中的元素，它才会执行它的代码：
```python
for x in gen:
    print(x, end=' ')  # 打印结果，空格
```
*输出结果如下：*
*Generating squares from 1 to 100
0 1 4 9 16 25 36 49 64 81 100 *

#### 3.2.6.1  生成器表达式
生成器表达式与列表、字典、集合的推导式很类似，创建一个生成器表达式，只需要将列表推导式的中括号替换为小括号即可：
```python
gen = (x ** 2 for x in range(100))
gen
```
*输出结果：<generator object <genexpr> at 0x0000022EC8E6E3C8>*

上面的代码与下面更为复杂的生成器是等价的：
```python
def make_gen():
    for x in range(100):
        yield x ** 2

gen = make_gen()
gen
```
*输出结果：<generator object make_gen at 0x0000022EC8E6E2C8>*

在很多情况下，生成器表达式可以作为函数参数用于替代列表推导式：
```python
sum((x ** 2 for x in range(100)))
```
*输出结果：328350*

```python
dict(((i, i ** 2) for i in range(5)))
```
*输出结果：{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}*

**注意：yeild a 相当于return a 只不过return很知足，得到a就结束。yeild得到a之后，继续执行代码，还可以继续得到b c d。**

```python
def yield_test_1(n):
    a = n
    yield a
    
    b = n * 2
    yield b
    
    c = n * 3
    yield c
    
    d = n * 4
    yield d

print(yield_test_1(10))
for i in yield_test_1(10):
    print(i)
```
*输出结果如下：*
*<generator object yield_test_1 at 0x0000022EC8E6E4C8>
10
20
30
40*

```python
# 点鞭炮
def yield_test(n):
    for i in range(n):
        # 生成一串小鞭炮，每个小鞭炮调用一次change函数
        yield change(i)
    print('end.')
    
def change(i):
    return i * 2

print(yield_test(6))

for i in yield_test(6):  # 使用for循环点鞭炮，含有第1个end
    print(i, 'in for.')
    
sum(yield_test(6))      # 用sum点鞭炮，含有第2个end
```
*输出结果如下：*
*<generator object yield_test at 0x0000022EC8E6EF48>
0 in for.
2 in for.
4 in for.
6 in for.
8 in for.
10 in for.
end.
end.
30*

#### 3.2.6.2  itertools模块
1.groupby(iterable[，keyfunc分组依据])
```python
from itertools import groupby

test = [(1, 5),(1, 4), (1, 3), (1,2), (2, 4), (2, 3), (3, 5)]
temp = groupby(test, key=lambda x: x[0])
# temp为一个迭代器
# x就是一个元组如(1, 5)
# key=x[0]按元组的0位元素分组

print(temp)
for a, b in temp:  # a为分组依据x[0]，b为分组后的结果
    print(a,list(b))
```
*输出结果如下：*
*<itertools.groupby object at 0x0000022ECA829DC8>
1 [(1, 5), (1, 4), (1, 3), (1, 2)]
2 [(2, 4), (2, 3)]
3 [(3, 5)]*

```python
from itertools import groupby

names = ['Alan', 'Adam', 'Wes', 'Will', 'Albert', 'Steven']
first_letter = lambda x: x[0] # 分组依据：打头元素分组
for letter, names in groupby(names, first_letter):
    print(letter, list(names))
```
*输出结果如下：*
*A ['Alan', 'Adam']
W ['Wes', 'Will']
A ['Albert']
S ['Steven']*

2.combinations(iterable, k)组合
```python
from itertools import combinations

test = combinations([1, 2, 3, 4], 3) # 将序列中取3个数字进行组合
for x in test:
    print(x)
```
*输出结果如下：*
*(1, 2, 3)
(1, 2, 4)
(1, 3, 4)
(2, 3, 4)*

2.permutations(iterable, k)排列
```python
from itertools import permutations

test = permutations([1, 2, 3, 4], 3)  # 将序列中取3个数字进行排列
for x in test:
    print(x)
```
*输出结果如下：*
*(1, 2, 3)
(1, 2, 4)
(1, 3, 2)
(1, 3, 4)
(1, 4, 2)
(1, 4, 3)
(2, 1, 3)
(2, 1, 4)
(2, 3, 1)
(2, 3, 4)
(2, 4, 1)
(2, 4, 3)
(3, 1, 2)
(3, 1, 4)
(3, 2, 1)
(3, 2, 4)
(3, 4, 1)
(3, 4, 2)
(4, 1, 2)
(4, 1, 3)
(4, 2, 1)
(4, 2, 3)
(4, 3, 1)
(4, 3, 2)*

3.product (*iterables, repeat=1) 多组之间组合
```python
from itertools import product

test = product('xy', 'abc')
for x in test:
    print(x)
```
*输出结果如下：*
*('x', 'a')
('x', 'b')
('x', 'c')
('y', 'a')
('y', 'b')
('y', 'c')*

### 3.2.7  错误和异常处理
在数据分析应用中，很多函数只能处理特定的输入。<br>
例如，Python的float函数可以将字符串转换为浮点数字，但是对不正确的输入会产生ValueError:
```python
float('1.2345')
```
*输出结果：1.2345*

```python
float('something')
```
*输出结果如下：*
```python
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-91-2649e4ade0e6> in <module>
----> 1 float('something')

ValueError: could not convert string to float: 'something'
```

假设我们想要在float函数运行失败时可以优雅地返回输入参数。我们可以通过将float函数写入一个try/except代码段来实现:<br>
1.except  抛出所有异常
```python
def attempt_float(x):
    try:
        return float(x)
    except:
        return x

attempt_float(1.2345)
```
*输出结果：1.2345*

```python
attempt_float('something')
```
*输出结果：'something'*

2.except  抛出指定异常
```python
float('something')
```
*输出结果如下：*

```python
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-101-1979c903cb56> in <module>
      1 # 2.except  抛出指定异常
----> 2 float('something')

ValueError: could not convert string to float: 'something'
```

```python
def attempt_float(x):
    try:
        return float(x)
    except ValueError:
        return x

attempt_float(1.2345)
```
*输出结果：1.2345*

```python
attempt_float('something')
```
*输出结果：'something'*

```python
float((1, 2))
```
*输出结果如下：*

```python
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-108-82f777b0e564> in <module>
----> 1 float((1, 2))

TypeError: float() argument must be a string or a number, not 'tuple'
```

```python
def attempt_float(x):
    try:
        return float(x)
    except (ValueError, TypeError):
        return x

attempt_float(1.2345)
```
*输出结果：1.2345*

```python
attempt_float((1, 2))
```
*输出结果：(1, 2)*

```python
attempt_float('something')
```
*输出结果：'something'*

***
else：try中的代码块执行成功，执行else代码块。<br>
finally：不管try中代码块执行成不成功，都执行finally代码块。<br>
```python
try:
     1 / 0
except ZeroDivisionError: # 指定错误，多个用元组
     print("Get AError")
except:
     print("exception")  # 所有报错
else:
     print("else") # try 成功执行的
finally:
     print("finally") # 成不成功都执行的
```
*输出结果如下：*
*Get AError
finally*

else语句的存在必须以except X或者except语句为前提
```python
try:
    1 / 0
else:
    print('else')
```
*输出结果：*

```python
  File "<ipython-input-113-cc3cc8b04a74>", line 3
    else:
       ^
SyntaxError: invalid syntax
```
