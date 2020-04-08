### 2.3.3控制流
#### 2.3.3.1 if、elif和else
一个if语句可以接多个elif代码块和一个else代码块，如果所有的elif条件均为False,则执行else代码块：
```python
x = input("PLease enter a number: ")
x = int(x)
if x < 0:
    print("It's negative")
elif x == 0:
    print('Equal to zero')
elif 0 < x < 5:
    print('Positive but smaller than 5')
else:
    print('Positive and larger than or equal to 5')
```
*输出结果：<br>
PLease enter a number: 3<br>
Positive but smaller than 5*

条件判断是从左到右的并且在and或or两侧的条件会有“短期”现象：
```python
a = 5; b = 7
c = 8; d = 4
# c>d是不会去判断的，因为第一个比较判断的值为True
if a < b or c > d:
    print('Made it')
```
*输出结果：Made it*

链式比较也是可以的:
```python
4 > 3 > 2 > 1
```
*输出结果：True*

#### 2.3.3.2 for循环
for循环用于遍历一个集合（例如列表或元组）或一个迭代器。标准的for循环语法如下：
```python
for value in collection:
    # 用值做什么
```
使用continue关键字可以跳过continue后面的代码进入下一次循环<br>
考虑以下代码，对列表中的非None值进行累加
```python
sequence = [1, 2, None, 4, 5]
total = 0
for value in sequence:
    if value == None:
        continue
    total += value
total
```
*输出结果：12*

***
使用break关键字可以结束一个for循环<br>
以下代码会对列表元素累加，直到5出现:
```python
sequence = [1, 2, 0, 4, 6, 5, 2, 1]
total_until_5 = 0
for value in sequence:
    if value == 5:
        break
    total_until_5 += value
total_until_5
```
*输出结果：13*

***
break关键字只结束最内层的for循环；最外层的for循环会继续运行:
```python
for i in range(4):
    for j in range(4):
        if j > i:
            break
        print((i, j))
```
*输出结果：<br>
(0, 0)<br>
(1, 0)<br>
(1, 1)<br>
(2, 0)<br>
(2, 1)<br>
(2, 2)<br>
(3, 0)<br>
(3, 1)<br>
(3, 2)<br>
(3, 3)<br>*

如果集合或迭代器中的元素是一个列表（比如元组或列表），它们可以在for循环语句中很方便地通过拆包变为变量：
```python
for a, b, c in iterator:
    # 做些什么
```

#### 2.3.3.3  while循环
while循环会在条件符合时一直执行代码块，直到条件判断为False或显示地以break结尾才结束：
```python
x = 256
total = 0
while x > 0:
    if total > 500:<br>
        break<br>
    total += x
    x = x // 2
total
```
*输出结果：504*

#### 2.3.3.4 pass
pass就是Python中的“什么都不做”的语句。它用于在代码段中表示不执行任何操作（或者是作为还没有实现的代码占位符）。
```python
x = input('please enter a number: ')
x = eval(x)
if x < 0:
    print('negative!')
elif x == 0:
    pass
else:
    print('positive!')
```
*输出结果：<br>
please enter a number: 999.56<br>
positive!*

#### 2.3.2.5 range
range函数返回一个迭代器，该迭代器生成一个等差整数序列：
```python
range(10)
```
*输出结果：range(0, 10)*

```python
# 数字列表
list(range(10))
```
*输出结果：[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]*

<br>
起始、结尾、步进（可以是负的）可以传参给range函数
```python
list(range(0, 20, 2))
```
*输出结果：[0, 2, 4, 6, 8, 10, 12, 14, 16, 18]*

```python
list(range(5, 0, -1))
```
*输出结果：[5, 4, 3, 2, 1]*

range常用于根据序列的索引遍历序列：
```python
seq = [1, 2, 3, 4]
sum = 0
for i in range(len(seq)):
    val = seq[i]
    sum += val
sum
```
*输出结果：10*

计算0到99,999中可以被3或5整除的整数相加：
```python
sum = 0
for i in range(100000):
    if i % 3 == 0 or i % 5 == 0:
        sum +=i
sum
```
*输出结果：2333316668*

#### 2.3.3.6 三元表达式
Python中的三元表达式允许你将一个if-else代码块联合起来，在一行代码或一个语句中生成数据。语法如下：
```python
value = true-expr if condition else false-expr
```

此处的true-expr和false-expr可以是任意Python表达式，它与以下更详细的代码效果一致：
```python
if condition:
    value = true-expr
else:
    value = false-expr
```

以下是具体的示例：
```python
x = 5
'None-negative' if x >= 0 else 'Negative'
```
*输出结果：'None-negative'*
