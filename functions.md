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
