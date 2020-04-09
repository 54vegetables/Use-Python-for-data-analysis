## 3.3  文件与操作系统
打开文件进行读取或写入，需要使用内建函数open和绝对、相对路径
```python
path = 'segismundo.txt'
f = open(path)
```
默认情况下，文件时以只读模式'r'打开的。之后可以向处理列表一样处理文件f，并遍历f中的行内容：
```python
for line in f:
    pass
```
当你使用open来创建文件对象时，在结束操作时显式地关闭文件是非常重要的。<br>
关闭文件会将资源释放回操作系统：
```python
f.close()
```

另一种更简单的关闭文件的方式就是使用with语句：
```python
with open(path) as f:
    for line in f:
        pass
```
**使用with语句，文件会在with代码块结束后自动关闭**

***
读取西班牙的一首诗
```python
# !/user/bin/python
# -*- encoding: utf-8 -*-
# 方法一
lines = [x.rstrip() for x in open(path, encoding='utf-8')]
for line in lines:
    print(line)
```
*输出结果如下：*
*Sueña el rico en su riqueza,
que más cuidados le ofrece;

sueña el pobre que padece
su miseria y su pobreza;

sueña el que a medrar empieza,
sueña el que afana y pretende,
sueña el que agravia y ofende,

y en el mundo, en conclusión,
todos sueñan lo que son,
aunque ninguno lo entiende.*

方法二：逐行读取
```python
f = open(path, encoding='utf-8')
for line in f:
    print(line.rstrip())
f.close()
```
*输出结果如下：*
*Sueña el rico en su riqueza,
que más cuidados le ofrece;

sueña el pobre que padece
su miseria y su pobreza;

sueña el que a medrar empieza,
sueña el que afana y pretende,
sueña el que agravia y ofende,

y en el mundo, en conclusión,
todos sueñan lo que son,
aunque ninguno lo entiende.*

方法三：使用readlines()读取文件的每一行并将其存储在一个列表中
```python
f = open(path, encoding='utf-8')
lines = f.readlines()
for line in lines:
    print(line.rstrip())
f.close()
```
*输出结果如下：*
*Sueña el rico en su riqueza,
que más cuidados le ofrece;

sueña el pobre que padece
su miseria y su pobreza;

sueña el que a medrar empieza,
sueña el que afana y pretende,
sueña el que agravia y ofende,

y en el mundo, en conclusión,
todos sueñan lo que son,
aunque ninguno lo entiende.*

read返回文件中一定量的字符，构成字符的内容是由文件的编码决定的（例如UTF-8）。<br>
read方法通过读取的字节数来推进文件句柄的位置：
```python
f = open(path, encoding='utf-8')
f.read(10)
```
*输出结果：'Sueña el r'*

或者在二进制模式下打开文件读取简单的原生字节：
```python
f2 = open(path, 'rb')
f2.read(10)
```
*输出结果：b'Sue\xc3\xb1a el '*

tell方法：给出句柄当前的位置：
```python
f.tell()
```
*输出结果：11*

```python
f2.tell()
```
*输出结果：10*

seek方法：将句柄位置改变到文件中特定的字节
```python
f.seek(3)
```
*输出结果：3*

```python
f.read(1)  # 改变句柄位置后，向后读取1个字节
```
*输出结果：'ñ'*

sys模块的getdefaultencoding()函数检查文件的默认编码方式：
```python
import sys
sys.getdefaultencoding()
```

**之后，请牢记关闭文件**
```python
f.close()
f2.close()
```

创建一个没有空行的prof_mod.py:
```python
with open('tmp.txt', 'w') as handle:
    # writelines函数将字符串序列写入文件
    handle.writelines(x for x in open(path) if len(x) > 1)

with open('tmp.txt', encoding='utf-8') as f:
    lines = f.readlines()  # 返回文件中行内容的列表

for line in lines:
    print(line.rstrip())
```
*输出结果如下：*
*Sueña el rico en su riqueza,
que más cuidados le ofrece;
sueña el pobre que padece
su miseria y su pobreza;
sueña el que a medrar empieza,
sueña el que afana y pretende,
sueña el que agravia y ofende,
y en el mundo, en conclusión,
todos sueñan lo que son,
aunque ninguno lo entiende.*

### 3.3.1 字节与Unicode文件
UTF-8是一种可变长度的Unicode编码，因此从文件中请求一定量的字符时，
Python从文件中读取了足够长的字节(可以少至10个字节，也可以多至40个字节)，并进行了解码。
```python
with open(path, encoding='utf-8') as f:
    chars = f.read(10)
chars
```
*输出结果：'Sueña el r'*

使用'rb'模式代替，则read请求提取了一定量的字节：
```python
with open(path, 'rb') as f:
    data = f.read(10)
data
```
*输出结果b'Sue\xc3\xb1a el '*

根据文本编码，你可能会将字节解码为str对象，但是只有每个已编码的Unicode字符是完整的情况下，才能进行解码：
```python
data.decode('utf-8')
```
*输出结果：'Sueña el '*

```python
data[:4].decode('utf-8')
```
*输出结果如下：*

```python
---------------------------------------------------------------------------
UnicodeDecodeError                        Traceback (most recent call last)
<ipython-input-46-8d4d1d2bf343> in <module>
----> 1 data[:4].decode('utf-8')

UnicodeDecodeError: 'utf-8' codec can't decode byte 0xc3 in position 3: unexpected end of data
```

利用open方法的选项参数encoding,Python提供了一种方便的方法将文件内容从Unicode编码转换为其他类型的编码：
```python
# !/user/bin/python
# -*- encoding: utf-8 -*-
sink_path = 'sink.txt'

with open(path, encoding='utf-8') as source:  # 原始文件是utf-8格式，采用utf-8加密方式读取文件
    with open(sink_path, 'w', encoding='iso-8859-1') as sink:  # 写入的文件编码方式是iso-8859-1
        sink.write(source.read())

with open(sink_path, encoding='iso-8859-1') as f:   # 读取编码格式为iso-8859-1的文件
    print(f.read(10))
```
*输出结果:Sueña el r*

除了二进制模式，在打开文件时使用seek要当心。如果文件的句柄位置恰好在一个Unicode符号的字节中间时，后续的读取会导致错误：
```python
f = open(path, encoding='utf-8')
f.read(5)
```
*输出结果:'Sueña'*

```python
f.seek(4)  # 句柄在ñ（Unicode符号的字节位置，后续读取会导致错误）
```
*输出结果：4*

```python
f.read(1)
```
*输出结果如下：*

```python
---------------------------------------------------------------------------
UnicodeDecodeError                        Traceback (most recent call last)
<ipython-input-71-5a354f952aa4> in <module>
----> 1 f.read(1)

D:\Anaconda3\lib\codecs.py in decode(self, input, final)
    320         # decode input (taking the buffer into account)
    321         data = self.buffer + input
--> 322         (result, consumed) = self._buffer_decode(data, self.errors, final)
    323         # keep undecoded input until the next call
    324         self.buffer = data[consumed:]

UnicodeDecodeError: 'utf-8' codec can't decode byte 0xb1 in position 0: invalid start byte
```
