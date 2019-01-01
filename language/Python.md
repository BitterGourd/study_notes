## 概述

##### python 介绍

> Python 由 Guido van Rossum（吉多·范罗苏姆，人称龟叔）创造
>
> Python 是一门解释型语言，不会在执行前对代码进行编译，而是在执行的同时一边执行一边编译
>
> Python 的设计哲学强调代码的可读性和简洁的语法，尤其是使用空格缩进划分代码块，而非使用大括号或者关键词

##### python 解释器

> CPython（官方）
> ​	用 C 语言编写的 Python 解释器
>  PyPy
> ​	用 Python 语言编写的 Python 解释器
> IronPython
> ​	用 .Net 编写的 Python 解释器
> Jython
> ​	用 Java 编写的 Python 解释器



## 基础语法

##### 编码

​	默认情况下，Python 3 源码文件以 UTF-8 编码，所有字符串都是 unicode 字符串。当然也可以在源码文件头部指定不同的编码：

```python
# -*- coding: cp-1252 -*-
```

​	上述定义允许在源文件中使用 Windows-1252 字符集中的字符编码。

##### 标识符

> 在 Python 中所有可以自主命名的内容都属于标识符
> 比如：变量名、函数名、类名

```python
# 1.标识符中可以含有字母、数字、_，但是不能使用数字开头
# 	例子：a_1 _a1 _1a
# 2.标识符不能是 Python 中的关键字和保留字
# 	也不建议使用 Python 中的函数名作为标识符,因为这样会导致函数被覆盖
# 3.命名规范：
# 	在 Python 中注意遵循两种命名规范：
#   	1)下划线命名法 => 变量名、函数名
#       	所有字母小写，单词之间使用_分割
#           max_length min_length hello_world xxx_yyy_zzz
#       2)帕斯卡命名法（大驼峰命名法） => 类名
#       	首字母大写，每个单词开头字母大写，其余字母小写
#           MaxLength MinLength HelloWorld XxxYyyZzz  
#       
# 如果使用不符合标准的标识符，将会报错 SyntaxError: invalid syntax    
```

##### 变量

```python
# 1.Python 中使用变量，不需要声明，直接为变量赋值即可
a = 10

# 2.不能使用没有进行过赋值的变量
#   如果使用没有赋值过的变量，会报错 NameError: name 'b' is not defined
# print(b)

# 3.Python 是一个动态类型的语言，可以为变量赋任意类型的值，也可以任意修改变量的值
a = 'hello'
print(a)
```

##### 保留字

> 即关键字，Python 的标准库提供了一个 keyword 模块，可以输出当前版本的所有关键字

```python
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

##### 注释

> 单行注释以 **#** 开头
>
> 多行注释可以用多个 # 号，还有 ''' ''' 和 """ """

##### 输出语句

```python
a = 123
# print("a = " + a) # 这种写法在 Python 中不常见
print('a =',a)      # 方式一，结果为 a = 123，逗号间自动添加空格
print('a = %d'%a)   # 方式二
print(f'a = {a}')   # 方式三

# 注意：print 语句括号里有可选参数 end，如果设置 end = ''，则表示不换行
print(a, end='||') # 123||
```

##### 行与缩进

> Python 最具特色的就是使用缩进来表示代码块，不需要使用大括号 {}
>
> 缩进的空格数是可变的，但是同一个代码块的语句必须包含相同的缩进空格数，否则会导致运行错误

```python
if True:
    print ("True")
else:
  print ("False")    # 缩进不一致，会导致运行错误
# IndentationError: unindent does not match any outer indentation level
```

> Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠 \ 来实现多行语句
>
> 在 [], {}, 或 () 中的多行语句，不需要使用反斜杠 \

```python
total = item_one + \
        item_two + \
        item_three

total = ['item_one', 'item_two', 'item_three',
        'item_four', 'item_five']
```

##### type 类型检查

> 该函数会将检查的结果作为返回值返回，可以通过变量来接收函数的返回值

```python
a = 123
print(type(a)) # <class 'int'>
```

##### 类型转换

> 类型转换四个函数 int()、float()、str()、bool()

```python
# =========== 1.int() 将其他的对象转换为整型 ===========
#   布尔值：True -> 1   False -> 0
#   浮点数：直接取整，省略小数点后的内容
#   字符串：合法的整数字符串，直接转换为对应的数字
#          如果不是一个合法的整数字符串，则报错
#		   ValueError: invalid literal for int() with base 10: '11.5'
#   对于其他不可转换为整型的对象，直接抛出异常 ValueError
# 注意：int() 函数不会对原来的变量产生影响，它是对象转换为指定的类型并将其作为返回值返回
# 	   如果希望修改原来的变量，则需要对变量进行重新赋值

# =========== 2.float() 将其他的对象转换为浮点数 ===========
# float() 和 int()基本一致，不同的是它会将对象转换为浮点数

# =========== 3.str() 可以将对象转换为字符串 ===========
#  True -> 'True'
#  False -> 'False'
#  123 -> '123'

# =========== 4.bool() 将对象转换为布尔值 ===========
# 任何对象都可以转换为布尔值
# 对于所有表示空性的对象都会转换为 False，其余的转换为 True
# 表示空性的对象: 0、None、''
```

##### 运算符

```python
############################ 1.算术运算符 ############################
+  加法运算符（如果是两个字符串之间进行加法运算，则会进行拼串操作）
-  减法运算符
*  乘法运算符（如果将字符串和数字相乘，则会对字符串进行复制操作，将字符串重复指定次数）
/  除法运算符，运算时结果总会返回一个浮点类型
// 整除，只会保留计算后的整数位，总会返回一个整型
** 幂运算，求一个值的几次幂
%  取模，求两个数相除的余数
	
############################ 2.赋值运算符 ############################
= 将等号右侧的值赋值给等号左侧的变量
+=   a += 5  等价于 a = a + 5 
-=   a -= 5  等价于 a = a - 5 
*=   a *= 5  等价于 a = a * 5 
**=  a **= 5 等价于 a = a ** 5 
/=   a /= 5  等价于 a = a / 5 
//=  a //= 5 等价于 a = a // 5 
%=   a %= 5  等价于 a = a % 5
	
############################ 3.关系运算符 ############################
# 关系运算符用来比较两个值之间的关系，总会返回一个布尔值
# 如果关系成立，返回True，否则返回False
	>      比较左侧值是否大于右侧值
	>=     比较左侧的值是否大于或等于右侧的值
	<      比较左侧值是否小于右侧值
	<=     比较左侧的值是否小于或等于右侧的值
	==     比较两个对象的值是否相等，比较的是对象的值，而不是 id
	!=     比较两个对象的值是否不相等，比较的是对象的值，而不是 id
	is     比较两个对象是否是同一个对象，比较的是对象的 id
	is not 比较两个对象是否不是同一个对象，比较的是对象的 id

ps : 在 Python 中，关系运算符可以连着使用
result = 1 < 2 < 3 # 相当于 1 < 2 and 2 < 3
	
############################ 4.逻辑运算符 ############################
not 逻辑非
	not 可以对符号右侧的值进行非运算
		对于布尔值，非运算会对其进行取反操作，True 变 False，False 变 True
        对于非布尔值，非运算会先将其转换为布尔值，然后再取反 
and 逻辑与
	and 可以对符号两侧的值进行与运算
		只有在符号两侧的值都为 True 时，才会返回 True，只要有一个 False 就返回 False
		Python 中的与运算是短路的与，如果第一个值为 False，则不再看第二个值  
or 逻辑或
	or 可以对符号两侧的值进行或运算
		或运算两个值中只要有一个 True，就会返回 True
		Python 中的或运算是短路的或，如果第一个值为 True，则不再看第二个值
		
############################ 5.条件运算符 ############################
语法： 语句1 if 条件表达式 else 语句2
	如果条件表达式结果为 True，则执行语句1，并返回执行结果
	如果条件表达式结果为 False，则执行语句2，并返回执行结果
```

##### 异常

```python
############################ try ... except ############################
try:
    # try中放置的是有可能出现错误的代码
    print(10/0)
except [exception_name [as Alias]]:
    # 放置出错以后处理措施
[except [exception_name [as Alias]]:
    # 放置出错以后处理措施]
[else:
    # 程序正常执行没有错误时的处理]
[finally :
    # 无论是否出现异常，都会执行]
 
# 如果 except 后不跟任何的内容，则此时它会捕获到所有的异常
# 如果在 except 后跟着一个异常的类型，那么此时它只会捕获该类型的异常
# Exception 是所有异常类的父类，所以如果 except 后跟的是 Exception，也会捕获到所有的异常
# 可以在异常类后边跟着一个 as xx，此时 xx 就是异常对象

############################ raise 抛出异常 ############################
# raise 用于向外部抛出异常，后边可以跟一个异常类，或异常类的实例
# raise Exception    
# 抛出异常的目的，告诉调用者这里调用时出现问题，希望调用者自己处理
def add(a,b):
	if a < 0 or b < 0:
		raise Exception('两个参数中不能有负数！') 
 
############################ 自定义异常 ############################
# 只需继承 Exception 即可
class MyError(Exception):
    pass
```



## 流程控制

##### 条件判断语句

```python
if 条件表达式 : 
	代码块  
# ===========================
if 条件表达式 :
	代码块
else :
	代码块 
# ===========================
if 条件表达式 :
	代码块
elif 条件表达式 :
	代码块
else :
	代码块
```

##### while 循环

```python
while 条件表达式 :
	代码块
[else :
	代码块]
```

##### for循环

> for 循环可以遍历任何序列，如一个列表或者一个字符串

```python
for s in 'hello':
    print(s)
```

##### break 和 continue

> break 可以用来立即退出循环语句（包括else）
>
> continue 可以用来跳过当次循环
>
> break 和 continue 都是只对离他最近的循环起作用
>
> pass 是用来在判断或循环语句中占位的，不做任何事情，防止报错



## 数据类型

> Numeric Types                —  int, float, complex
>
> Sequence Types              —  list, tuple, range
>
> Text Sequence Type        —  str
>
> Binary Sequence Types  —  bytes, bytearray, memoryview
>
> Set Types                          —  set, frozenset
>
> Mapping Types                —  dict

#### Numbers（数字）

##### 简介

```python
# Python 数值分成了三种：整数、浮点数（小数）、复数，布尔值（bool）是整数的子类型
# 在 Python 中所有的整数都是 int 类型，所有的小数都是 float 类型
# Python 中的整数的大小没有限制，可以是一个无限大的整数
# 对浮点数进行运算时，可能会得到一个不精确的结果
# 如果数字的长度过大，可以使用下划线作为分隔符
c = 123_456_789

# 二进制 0b开头
c = 0b10 # 二进制的 10，十进制为 2
# 八进制 0o开头
c = 0o10 # 八进制的 10，十进制为 8
# 十六进制 0x开头
c = 0x10 # 十六进制的 10，十进制为 16
```

##### 运算

| Operation    | Result                        |
| ------------ | ----------------------------- |
| x + y        | 加                            |
| x - y        | 减                            |
| x * y        | 乘                            |
| x / y        | 除（总是浮点数）              |
| x // y       | 商                            |
| x % y        | 取模（余数）                  |
| -x           | 负数                          |
| +x           | 正数                          |
| abs(x)       | 绝对值                        |
| int(x)       | 转换成整数                    |
| float(x)     | 转换成浮点数                  |
| divmod(x, y) | 转换成 (x // y, x % y) 键值对 |
| pow(x, y)    | x 的 y 次方                   |
| x ** y       | x 的 y 次方                   |



#### String（字符串）

> 单引号：允许嵌入式双引号
>
> 双引号：允许嵌入单引号
>
> 三引号：三个单引号或三个双引号，三引号字符串可以跨越多行，所有相关的空格都将包含在字符串文字中

##### 转义字符

> 在需要在字符中使用特殊字符时，Python 用 \ 转义字符

| 转义字符     | 描述            |
| ------------ | --------------- |
| \ (在行尾时) | 续行符          |
| \b           | 退格(Backspace) |
| \000         | 空              |
| \n           | 换行            |
| \u           | Unicode 编码    |
| \v           | 纵向制表符      |
| \t           | 横向制表符      |
| \r           | 回车            |
| \f           | 换页            |

##### 运算符

| 操作符  | 描述                                                 | 实例                                     |
| ------- | ---------------------------------------------------- | ---------------------------------------- |
| +       | 字符串连接，不能和其他的类型进行加法运算，否则会报错 | ‘hello’ + ‘python’ => 'hellopython'      |
| *       | 重复输出字符串                                       | ‘hello’ * 2 => 'hellohello'              |
| [n]     | 通过索引获取字符串中字符                             | 'hello'[1] => ‘e’, 'hello'[-1] => ‘o’    |
| [l :r ] | 截取字符串中的一部分，遵循左闭右开原则               | 'hello'[0:1] => 'h','hello'[-2:] => 'lo' |
| in      | 如果字符串中包含给定的字符返回 True                  | 'h' in 'hello' => True                   |
| not in  | 如果字符串中不包含给定的字符返回 True                | 'a' not in 'hello' => True               |
| %       | 格式字符串                                           |                                          |

##### 格式化字符串

| 占位符 | 替换内容     |
| ------ | ------------ |
| %d     | 整数         |
| %f     | 浮点数       |
| %s     | 字符串       |
| %x     | 十六进制整数 |

##### str 与 butes

> Python 对 bytes 类型的数据用带b前缀的单引号或双引号表示

```python
a = b'ABC'
b = 'ABC'
print(type(a)) # <class 'bytes'>，bytes 的每个字符只占一个字节
print(type(b)) # <class 'str'>，str 的每个字符就占一个字符
```

##### input 函数

> 用来获取用户的输入

```python
# input() 调用后，程序会立即暂停，等待用户输入
# 	用户输入完内容以后，点击回车程序才会继续向下执行
#   用户输入完成以后，其所输入的的内容会以返回值得形式返回
#   注意：input()的返回值是一个字符串
#   	 input()函数中可以设置一个字符串作为参数，这个字符串将会作为提示文字显示
content = input('请输入任意内容：')
print('用户输入的内容是:', content)

# input()也可以用于暂时阻止程序结束
```



#### List（列表）

> 列表是一个可变序列

##### 创建

> 通过 [] 来创建列表

```python
my_list = []       							        # 创建了一个空列表
my_list = [10]   							        # 创建一个只包含一个元素的列表
my_list = [10, 20]						            # 创建了一个有 2 个元素的列表
my_list = [10, 'hello', True, None, [1,2,3], print] # 列表中可以保存任意对象
```

##### 操作

> 适用于 List，Tuple，Range

| Operation           | Result                                                     |
| ------------------- | ---------------------------------------------------------- |
| x in s              | x 在 s 中返回 True，否则返回 False                         |
| x not in s          | x 不在 s 中返回 True，否则返回 False                       |
| s + t               | 将两个 s 拼接为一个新的序列                                |
| s * n or n * s      | s 重复的次数，返回新列表，即添加 n 个 s                    |
| s[i]                | 返回 s 的第 i 个元素，下标从 0 开始                        |
| s[i:j]              | 从 i 到 j 的切片，左闭右开                                 |
| s[i:j:k]            | 从 i 到 j 的切片，步长为 k，左闭右开                       |
| len(s)              | s 的长度                                                   |
| min(s)              | 返回 s 的最小值                                            |
| max(s)              | 返回 s 的最大值                                            |
| s.index(x[, i[,j]]) | s 中第一次出现 x 的索引（在索引 i 之后，j 之前，左闭右开） |
| s.count(x)          | s 中出现 x 的次数                                          |

##### 修改元素

```python
stus = ['孙悟空','猪八戒','哈哈','唐僧','蜘蛛精','白骨精']

# 1.通过索引修改元素
stus[2] = '沙和尚' # stus = ['孙悟空', '猪八戒', '沙和尚', '唐僧', '蜘蛛精', '白骨精']

# 2.通过切片修改列表,在给切片进行赋值时，只能使用序列
# stus = ['牛魔王', '红孩儿', '沙和尚', '唐僧', '蜘蛛精', '白骨精']
stus[0:2] = ['牛魔王','红孩儿'] # 使用新元素替换旧元素

# stus = ['牛魔王', '红孩儿', '二郎神', '沙和尚', '唐僧', '蜘蛛精', '白骨精']
stus[0:2] = ['牛魔王','红孩儿','二郎神'] # 比切片个数多时，直接插入

# stus = ['牛魔王', '孙悟空', '猪八戒', '哈哈', '唐僧', '蜘蛛精', '白骨精']
stus[0:0] = ['牛魔王'] # 向索引为0的位置插入元素

# 当设置了步长时，序列中元素的个数必须和切片中元素的个数一致，否则会报错：ValueError
# stus = ['牛魔王', '猪八戒', '红孩儿', '唐僧', '二郎神', '白骨精']
stus[::2] = ['牛魔王','红孩儿','二郎神']
```

##### 删除元素

```python
del stus[0]
del stus[0:2]
del stus[::2]
stus[1:3] = []
```

##### 方法

```python
test = [1,2,3,4]

# 1.append(element)，向列表的最后添加一个元素
test.append(5) # test = [1, 2, 3, 4, 5]

# 2.insert(index, element)，向列表的指定位置插入一个元素
test.insert(1, 10) # test = [1, 10, 2, 3, 4, 5]

# 3.extend(sequence)，使用新的序列来扩展当前序列，等价于 +=
test.extend([6,7]) # test = [1, 10, 2, 3, 4, 5, 6, 7]

# 4.pop(index)，删除并返回索引所在的元素，不加索引则删除最后一个元素
test.pop(1) # 10

# 5.remove(element)，删除指定的元素，如果相同值的元素有多个，只会删除第一个
test.remove(1) # test = [2, 3, 4, 5, 6, 7]

# 6.reverse()，反转列表
test.reverse() # test = [7, 6, 5, 4, 3, 2]

# 7.sort()，对列表中的元素排序
#   默认是升序排列，如果需要降序排列，需要传递一个 reverse = True 作为参数

# 8.clear()，清空序列
test.clear() # test = []
```

##### 遍历

```python
test = [1,2,3,4,5]

# 方式一（推荐）
for i in test:
    print(i)
    
# 方式二
i = 0
while i < len(test):
	print(test[i])
	i += 1
```



#### Tuple（元组）

> 元组是一个不可变序列

##### 创建

> 使用 () 创建元组

```python
my_tuple = () # 创建了一个空元组
my_tuple = (1,2,3,4,5) # 创建了一个5个元素的元组

# 当元组不是空元组时，括号可以省略，如果元组不是空元组，它里边至少要有一个逗号
my_tuple = 10,20,30,40
my_tuple = 40,

# 元组是不可变对象，不能为元组中的元素重新赋值
# my_tuple[3] = 10 # TypeError: 'tuple' object does not support item assignment
```

##### 操作

​	参考 List 的操作

##### 元组的解包

> 解包指就是将元组当中每一个元素都赋值给一个变量

```python
my_tuple = 10,20,30,40
a,b,c,d = my_tuple # a = 10, b = 20, c = 30, d = 40

# 在对一个元组进行解包时，变量的数量必须和元组中的元素的数量一致
# 也可以在变量前边添加一个 *，这样变量将会获取元组中所有剩余的元素
a, b, *c = my_tuple # a = 10, b = 20, c = [30, 40]
a, *b, c = my_tuple # a = 10, b = [20, 30], c = 40
*a, b, c = my_tuple # a = [10, 20], b = 30, c = 40

# 不能同时出现两个或以上的*变量
# *a , *b , c = my_tuple # SyntaxError: two starred expressions in assignment

# ps : 交换 a, b 的值，可以利用元组的解包
a = 100
b = 200
print(a, b) # 100 200
a, b = b, a
print(a, b) # 200 100
```



#### Sets（集合）

> set 对象是不同可哈希对象的无序集合

##### 创建

```python
# 方式一，使用 {} 创建
s = {1, 2, 3, 4}

# 方式二，使用 set() 函数创建
s = set() # 空集合
s = set(1, 2, 'a', 'b')
s = set({'a':1,'b':2,'c':3}) # 使用 set() 将字典转换为集合时，只会包含字典中的键
```

##### 操作

```python
# 1.x in s / x not in s 检查元素 x 是否在集合中，返回 True 或 False
# 2.len(s) 获取集合中元素的数量
# 3.add(elem) 向集合中添加元素

# 4.update(seq) 将一个集合中的元素添加到当前集合中,可以传递序列或字典作为参数，字典只会使用键
s1 = {1, 2, 3}
s2 = set('hello')
s1.update(s2)
print(s1) # {1, 2, 3, 'h', 'o', 'e', 'l'}

# 5.pop() 随机删除集合中的一个元素并返回
# 6.remove(elem) 删除集合中的指定元素
# 7.clear() 清空集合
# 8.copy() 对集合进行浅复制
# 9.difference(*others) 返回一个新集合，其中集合中的元素不在其他集合中
# 10.union(*others) 并集
# 11.intersection(*others) 交集 
```

#####  运算

> 在对集合做运算时，不会影响原来的集合，而是返回一个运算结果

```python
s1 = {1,2,3,4,5}
s2 = {3,4,5,6,7}

# 1.& 交集运算
result = s1 & s2 # {3, 4, 5}

# 2.| 并集运算
result = s1 | s2 # {1,2,3,4,5,6,7}

# 3.- 差集
result = s2 - s2 # {1, 2}

# 4.^ 异或集 获取只在一个集合中出现的元素
result = s1 ^ s2 # {1, 2, 6, 7}

# 5.<= 检查一个集合是否是另一个集合的子集
# 如果a集合中的元素全部都在b集合中出现，那么a集合就是b集合的子集，b集合是a集合超集
result = {1,2,3} <= {1,2,3,4,5} # True

# 6.< 检查一个集合是否是另一个集合的真子集
# 	如果超集 b 中含有子集 a 中所有元素，并且 b 中还有 a 中没有的元素
# 	则 b 就是 a 的真超集，a 是 b 的真子集

# 7.>= 检查一个集合是否是另一个的超集
# 8.>  检查一个集合是否是另一个的真超集
```



#### Dictionaries（字典）

> 映射是可变对象
>
> 字典是 Python 唯一的标准映射类型

##### 创建

> 字典的值可以是任意对象
> 字典的键可以是任意的不可变对象（int、str、bool、tuple ...），但是一般使用 str
> 字典的键是不能重复的，如果重复会替换值

```python
# 方式一，使用 {} 创建
my_dict = {} 										   # 创建了一个空字典
my_dict = {'name': '孙悟空', 'age': 18, 'gender': '男'} # 创建了一个有内容的字典

# 方式二，使用 dict() 函数创建
my_dict = dict(name='孙悟空', age=18, gender='男')
my_dict = dict([('name','孙悟饭'),('age',18)]) # 可以将一个包含有双值子序列的序列转换为字典
```

##### 操作

```python
my_dict = {'name': '孙悟空', 'age': 18, 'gender': '男'}

# 1.len(d) 获取字典中键值对的个数
print(len(my_dict)) # 3

# 2.key in d 检查字典中是否包含指定的键 / key not in d 检查字典中是否不包含指定的键
print('hello' in my_dict) # False

# 3.d[key] 根据键获取字典中的值,如果键不存在，会抛出异常 KeyError
print(my_dict['age']) # 18

# 4.get(key[, default]) 根据键获取字典中的值
#   若获取的键在字典中不存在，返回 None，也可指定默认值，获取不到值时会返回默认值
print(my_dict.get('name', '猪八戒')) # 孙悟空

# 5.d[key] = value 如果key存在则覆盖，不存在则添加
my_dict['name'] = 'sunwukong' # 修改字典的 key-value
my_dict['address'] = '花果山'  # 向字典中添加 key-value
# my_dict = {'name': 'sunwukong', 'age': 18, 'gender': '男', 'address': '花果山'}

# 6.del d[key] 删除 key 对应的键值对，如果 key 不存在，产生异常 KeyError
del my_dict['address'] # my_dict = {'name': 'sunwukong', 'age': 18, 'gender': '男'}

# 7.setdefault(key[, default]) 
#   如果 key 已经存在于字典中，则返回 key 的值，不会对字典做任何操作
#   如果 key 不存在，则向字典中添加这个 key，并设置 value = default，返回 key
result = my_dict.setdefault('name','猪八戒')    # result = '孙悟空'
# my_dict = {'name': 'sunwukong', 'age': 18, 'gender': '男'}
result = dmy_dict.setdefault('hello','猪八戒')  # result = '猪八戒'
# # my_dict = {'name': 'sunwukong', 'age': 18, 'gender': '男', 'hello': '猪八戒'}

# 8.update([other]) 将其他的字典中的 key-value 添加到当前字典中,如果有重复的 key，则更新值
d = {'a': 1, 'b': 2, 'c': 3}
d2 = {'d': 4, 'e': 5, 'f': 6, 'a': 7}
d.update(d2)
print(d) # {'a': 7, 'b': 2, 'c': 3, 'd': 4, 'e': 5, 'f': 6}

# 9.popitem() 随机删除字典中的一个键值对，一般都会删除最后一个键值对
#   删除之后，将删除的 key-value 作为返回值返回
#   返回的是一个元组，元组中有两个元素，第一个元素是删除的 key，第二个是删除的 value
# 当使用 popitem() 删除一个空字典时，会抛出异常 KeyError: 'popitem(): dictionary is empty'

# 10.pop(key[, default]) 根据 key 删除字典中的 key-value，返回被删除的 value
# 	 如果删除不存在的 key，会抛出异常
#    如果指定了默认值，再删除不存在的 key 时，不会报错，而是直接返回默认值

# 11.clear() 清空字典

# 12.copy() 对字典进行浅复制，复制以后的对象，和原对象是独立，修改一个不会影响另一个
# 注意：浅复制会简单复制对象内部的值，如果值也是一个可变对象，这个可变对象不会被复制
```

##### 遍历

```python
d = {'name':'孙悟空','age':18,'gender':'男'}

# 1.keys() 返回字典所有的 key
for k in d.keys():
    print(k, d[k])
    
# 2.values() 返回一个序列，序列中保存着字典所有的值
for v in d.values():
    print(v)
    
# 3.items() 返回一个序列，序列中包含的是 key-value 双值子序列
for k, v in d.items():
    print(k, '=', v)
```



## 函数式编程

#### 简介

```python
# 1.函数的定义
def 函数名（参数列表）:
    函数体

# 示例：
def hello() :
   print("Hello World!")

def area(width, height):
    return width * height

# 2.打印函数
print(area)		  # <function area at 0x0000015C4296C268>
print(type(area)) # <class 'function'>

# 注意：hello 是函数对象，hello() 调用函数；print 是函数对象，print() 调用函数
```



#### 参数

```python
# 1.定义形参时，可以为形参指定默认值
# 	指定了默认值以后，如果用户传递了参数则默认值没有任何作用
#   如果用户没有传递，则默认值就会生效
def fn(a = 5, b = 10):
    print('a =', a)
    print('b =', b)

fn(1, 2) # a = 1 b = 2
fn(1)    # a = 1 b = 10
fn()     # a = 5 b = 10

# 2.关键字参数，可以不按照形参定义的顺序去传递，而直接根据参数名去传递参数
fn(b=1, a=3) # a = 3 b = 1

# 3.不定长参数
# 	在定义函数时，可以在形参前边加上一个 *，这样这个形参将会获取到所有的实参
# 	它会将所有的实参保存到一个元组中
def sum(*nums):
    result = 0
    for n in nums :
        result += n
    print(result)
 
# * 形参只能接收位置参数，而不能接收关键字参数
# 可变参数不是必须写在最后，但是注意，带 * 的参数后的所有参数，必须以关键字参数的形式传递
# 第一个参数给 a，剩下的位置参数给 b 的元组，c 必须使用关键字参数
def fn2(a, *b, c):
	print('a =',a)
    print('b =',b)
    print('c =',c)
    
# ** 形参可以接收其他的关键字参数，它会将这些参数统一保存到一个字典中
#    	字典的 key 就是参数的名字，字典的 value 就是参数的值
# ** 形参只能有一个，并且必须写在所有参数的最后
def fn3(b, c, **a):
    print('a =', a, type(a))
    print('b =', b)
    print('c =', c)

fn3(b=1, d=2, c=3, e=10, f=20)
# a = {'d': 2, 'e': 10, 'f': 20} <class 'dict'>
# b = 1 c = 3

# 传递实参时，也可以在序列类型的参数前添加星号，这样他会自动将序列中的元素依次作为参数传递
# 这里要求序列中元素的个数必须和形参的个数的一致
def fn4(a, b, c):
    print('a =', a)
    print('b =', b)
    print('c =', c)
    
t = (10, 20, 30)
fn4(*t) # a = 10 b = 20 c = 30

# 通过 ** 来对一个字典进行解包操作
d = {'a':100,'b':200,'c':300}
fn4(**d) # a = 100 b = 200 c = 300
```



#### 返回值

> 通过 return 指定函数的返回值

```python
# return 后边跟什么值，函数就会返回什么值
# return 后边可以跟任意的对象，返回值甚至可以是一个函数
# 如果仅仅写一个 return 或者 不写 return，则相当于 return None 

def sum(*nums):
    result = 0
    for n in nums :
        result += n
    return result
```



#### 作用域

> 作用域（scope）: 指的是变量生效的区域

```python
#  全局作用域
#   - 全局作用域在程序执行时创建，在程序执行结束时销毁
#   - 所有函数以外的区域都是全局作用域
#   - 在全局作用域中定义的变量，都属于全局变量，全局变量可以在程序的任意位置被访问
   
#  函数作用域
#   - 函数作用域在函数调用时创建，在调用结束时销毁
#   - 函数每调用一次就会产生一个新的函数作用域
#   - 在函数作用域中定义的变量，都是局部变量，它只能在函数内部被访问
# 注意：在函数中为变量赋值时，默认都是为局部变量赋值
# 如果希望在函数内部修改全局变量，则需要使用 global 关键字来声明变量

#  变量的查找
#   - 当我们使用变量时，会优先在当前作用域中寻找该变量，如果有则使用，
#       如果没有则继续去上一级作用域中寻找，如果有则使用，
#       如果依然没有则继续去上一级作用域中寻找，以此类推
#       直到找到全局作用域，依然没有找到，则会抛出异常
#           NameError: name 'a' is not defined
```



#### 命名空间

> 命名空间（namespace）: 指的是变量存储的位置

```python
# 全局命名空间，用来保存全局变量。函数命名空间用来保存函数中的变量
# 命名空间实际上就是一个字典，是一个专门用来存储变量的字典

# locals() 用来获取当前作用域的命名空间
# 如果在全局作用域中调用 locals() 则获取全局命名空间
# 如果在函数作用域中调用 locals() 则获取函数命名空间
```



#### 高阶函数

> 接收函数作为参数，或者将函数作为返回值的函数是高阶函数

```python
def add(a, b):
    return a + b

def getSum(func, a, b):
    return func(a, b)

print(getSum(add, 1, 2)) # 3
```



#### 匿名函数

> 使用 lambda 表达式

```python
lambda x: x * x

# 等价于 =>

def f(x):
    return x * x
```



#### 闭包

> 通过闭包可以创建一些只有当前函数能访问的变量
> 可以将一些私有的数据藏到的闭包中

```python
def fn():
    a = 10
    # 函数内部再定义一个函数
    def inner():
        print('我是fn2' , a)
    # 将内部函数 inner作为返回值返回   
    return inner

# r 是一个函数，是调用 fn() 后返回的函数
# 这个函数是在 fn() 内部定义，并不是全局函数
# 所以这个函数总是能访问到fn()函数内的变量
r = fn()    
r() # 我是fn2 10

# 形成闭包的要件
#   ① 函数嵌套
#   ② 将内部函数作为返回值返回
#   ③ 内部函数必须要使用到外部函数的变量
```



#### 装饰器

> 在代码运行期间动态增加功能的方式，称之为“装饰器”（Decorator）

```python
def add(a, b):
    print(a + b)

def begin_end(old_func):
    def new_function(*args, **kwargs):
        print('开始执行~~~~')
        result = old_func(*args, **kwargs)
        print('执行结束~~~~')
        return result
    return new_function

f = begin_end(add)
f(10, 20)
# 结果：
# 开始执行~~~~
# 30
# 执行结束~~~~

# 像 begin_end() 这种函数我们就称它为装饰器
#   通过装饰器，可以在不修改原来函数的情况下来对函数进行扩展
#   在开发中，我们都是通过装饰器来扩展函数的功能的
# 在定义函数时，可以通过 @装饰器，来使用指定的装饰器，来装饰当前的函数
#   可以同时为一个函数指定多个装饰器，这样函数将会安装从内向外的顺序被装饰 
#   常见示例：classmethod() / staticmethod()
def decorator(old):
    def new_function(*args, **kwargs):
        print('decorator 装饰~开始执行~~~~')
        result = old(*args, **kwargs)
        print('decorator 装饰~执行结束~~~~')
        return result
    return new_function

@decorator
@begin_end
def say_hello():
    print('大家好~~~')

say_hello()
# decorator 装饰~开始执行~~~~
# 开始执行~~~~
# 大家好~~~
# 执行结束~~~~
# decorator 装饰~执行结束~~~~
```



#### 内置函数

##### abs(x)

> 返回数字的绝对值，参数可以是整数或浮点数
> 如果参数是复数，则返回其大小（浮点数表示）

##### filter((function, iterable)

> 从序列中过滤出符合条件的元素，保存到一个新的序列中

```python
l = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
r = filter(lambda i: i > 5, l)
print(list(r)) # [6, 7, 8, 9, 10]
```

##### hex(x)

> 将整数转换为带有前缀“0x”的小写十六进制字符串

```python
>>> hex(255)
'0xff'
>>> hex(-42)
'-0x2a'
```

##### id(object)

> 返回对象的 id

##### isinstance(object, classinfo)

> 判断 object 是否是 classinfo 的实例

```python
>>> isinstance('python',str)
True
>>> isinstance('python',int)
False
```

##### len(s)

> 返回对象的长度（项数）
> 参数可以是一个序列（字符串，字节，元组，列表或范围）或集合（字典，集合）

```python
>>> len('hello')
5
>>> len({'name':'Tom','age':20,'city':'beijing'})
3
```

##### map(function, iterable, ...)

> 可以对可迭代对象中的所有元素做指定的操作，然后将其添加到一个新的对象中返回

```python
l = [1, 2, 3, 4, 5]
r = map(lambda i: i ** 2, l)
print(list(r)) # [1, 4, 9, 16, 25]
```

##### range()

>  range(stop) -> range object
>
>  range(start, stop[, step]) -> range object

```python
for i in range(5):
	print(i) # 0，1，2，3，4
    
for i in range(5, 9):
	print(i) # 5，6，7，8
    
for i in range(0, 10, 3) : # 3 是步长
	print(i) # 0,3,6,9
```

##### sorted(iterable, *, key=None, reverse=False)

> 对序列进行排序并且不会影响原来的对象，而是返回一个新对象

```python
l = [2, 5, '1', 3, '6', '4']

print('排序前:', l)		# 排序前: [2, 5, '1', 3, '6', '4']
print(sorted(l, key=int)) # ['1', 2, 3, '4', 5, '6']
print('排序后:', l)		# 排序后: [2, 5, '1', 3, '6', '4']
```

##### zip(*iterables)

> 返回一个拉链的元组

```python
>>> x = [1, 2, 3]
>>> y = [4, 5, 6]
>>> zipped = zip(x, y)
>>> list(zipped)
[(1, 4), (2, 5), (3, 6)]

>>> x2, y2 = zip(*zip(x, y)) # 还原序列
>>> x == list(x2) and y == list(y2)
True
```



## 面向对象编程

##### 类的定义

```python
# 定义语法：
class 类名([父类]):
	代码块
    
class Person:
    name = 'Tom'
    def hello(self):
        # 在方法中不能直接访问类中的属性
        print('hello! I am %s' % self.name)

# 创建 Person 的实例
p = Person()
# 调用方法，对象.方法名()
p.hello() # hello! I am Tom
# 修改 p 的 name 属性
p.name = 'Lucy'
p.hello() # hello! I am Lucy
# 删除 p 的 name 属性
del p.name
print(p.name) # Tom
```

##### 属性和方法

> 属性：类属性、实例属性
>
> 方法：类方法、实例方法、静态方法

```python
""" ================= 类属性 ================= """
# 类属性，直接在类中定义的属性是类属性
#   类属性可以通过类或类的实例访问到
#   但是类属性只能通过类对象来修改，无法通过实例对象修改
class A:
    # 类属性
    count = 0
    
""" ================= 实例属性 ================= """
# 实例属性，通过实例对象添加的属性属于实例属性
#   实例属性只能通过实例对象来访问和修改，类对象无法访问修改
class Person:
    def __init__(self):
        # 实例属性，通过实例对象添加的属性属于实例属性
        #   实例属性只能通过实例对象来访问和修改，类对象无法访问修改
        self.name = 'Lucy'
        
""" ================= 实例方法 ================= """
#   在类中定义，以 self 为第一个参数的方法都是实例方法
#   实例方法在调用时，Python 会将调用对象作为 self 传入  
#   实例方法可以通过实例和类去调用
#       当通过实例调用时，会自动将当前调用对象作为 self 传入
#       当通过类调用时，不会自动传递 self，此时我们必须手动传递 self
class B:
    def test(self):
        print('这是test方法~~~ ', self) 
        
""" ================= 类方法 ================= """
# 在类内部使用 @classmethod 来修饰的方法属于类方法
# 类方法的第一个参数是 cls，也会被自动传递，cls 就是当前的类对象
#   类方法和实例方法的区别，实例方法的第一个参数是 self，而类方法的第一个参数是 cls
#   类方法可以通过类去调用，也可以通过实例调用，没有区别
class C:
    @classmethod
    def test(cls):
        print('这是 test 方法，它是一个类方法~~~ ', cls)
        
""" ================= 静态方法 ================= """
# 在类中使用 @staticmethod 来修饰的方法属于静态方法  
# 静态方法不需要指定任何的默认参数，静态方法可以通过类和实例去调用  
# 静态方法，基本上是一个和当前类无关的方法，它只是一个保存到当前类中的函数
# 静态方法一般都是一些工具方法，和当前类无关
class D:
     @staticmethod
    def test():
        print('test 方法执行了~~~')
```

##### 特殊方法

> 特殊方法，也称为魔术方法
>
> 特殊方法都是使用__开头和结尾的
>
> 特殊方法一般不需要我们手动调用，需要在一些特殊情况下自动执行

```python
# 1.__init__() 创建对象时调用，是类的构造方法
# 2.__str__()  在尝试将对象转换为字符串的时候调用，相当于 Java 的 toString 方法
# 3.__repr__() 在对当前对象使用 repr() 函数时调用，它的作用是指定对象在'交互模式'中直接输出的效果 # 4.__len__()  获取对象的长度
# 5.__slots__  限制实例的属性，使用元组绑定（括号可省略，以逗号隔开）
class Person(object):
    __slots__ = ('name',)
    def __init__(self, name, age):
        self.name = name
        self.age = age

p = Person('Tom',20) # AttributeError: 'Person' object has no attribute 'age'

# 6.__bool__(self) 指定对象转换为布尔值的情况
def __bool__(self):
        return self.age > 17
    
# 7.__gt__() 在对象做大于比较的时候调用，该方法的返回值将会作为比较的结果
# 	object.__lt__(self, other) 小于 <
# 	object.__le__(self, other) 小于等于 <=
# 	object.__eq__(self, other) 等于 ==
# 	object.__ne__(self, other) 不等于 !=
# 	object.__ge__(self, other) 大于等于 >= 
```

##### 对象的初始化

```python
""" ================= 创建对象的流程 ================= """
# p = Person() 的运行流程
#   1.创建一个变量
#   2.在内存中创建一个新对象
#   3.__init__(self) 方法执行 => 构造方法
#   4.将对象的 id 赋值给变量

# init 会在对象创建以后立刻执行
# init 可以用来向新创建的对象中初始化属性
# 调用类创建对象时，类后边的所有参数都会依次传递到 init() 中
class Person :
    # self 代表类的实例，而非类
	def __init__(self, name):
		# print(self) # <__main__.Person object at 0x000002EFA13A2400>
		# 通过 self 向新建的对象中初始化属性
		self.name = name
```

##### 封装

> 封装指的是隐藏对象中一些不希望被外部所访问到的属性或方法

```python
""" ================= 私有属性 ================= """
# 可以为对象的属性使用双下划线开头，__xxx
# 双下划线开头的属性，是对象的隐藏属性，隐藏属性只能在类的内部访问，无法通过对象访问
# 其实隐藏属性只不过是 Python 自动为属性改了一个名字
# 实际上是将名字修改为了：_类名__属性名 比如 __name -> _Person__name
class Person:
    def __init__(self, name):
        self.__name = name

    def get_name(self):
        return self.__name

    def set_name(self, name):
        self.__name = name

p = Person('Tom')
print(p._Person__name) # Tom

p._Person__name = 'Lucy'
print(p._Person__name) # Lucy

# 使用__开头的属性，实际上依然可以在外部访问，所以这种方式一般不用
# 一般会将一些私有属性（不希望被外部访问的属性）以_开头
# 一般情况下，使用_开头的属性都是私有属性，没有特殊需要不要修改私有属性

""" ================= 私有属性 ================= """
# property 装饰器，用来将一个 get 方法，转换为对象的属性
# 添加为 property 装饰器以后，我们就可以像调用属性一样使用 get 方法
# 使用 property 装饰的方法，必须和属性名是一样的
class Person:
	# ...
    
    @property    
    def name(self):
        print('getter 方法执行了~~~')
        return self._name

    # setter方法的装饰器：@属性名.setter
    @name.setter    
    def name(self , name):
        print('setter 方法调用了')
        self._name = name
```

##### 继承

> 通过继承可以直接让子类获取到父类的方法或属性，包括特殊方法

```python
class Person:
    # ...
    
# 继承 Person 类
class Student(Person):
    # ...

# 在创建类时，如果省略了父类，则默认父类为 object
# object 是所有类的父类，所有类都继承自 object

# issubclass() 检查一个类是否是另一个类的子类
print(issubclass(Person, object)) # True
print(issubclass(Student, Person)) #3 True

# isinstance() 用来检查一个对象是否是一个类的实例
#   如果这个类是这个对象的父类，也会返回 True
#   所有的对象都是 object 的实例
p = Person()
print(isinstance(p, Person))	  # True
print(isinstance(Person, object)) # True

""" ====================== 重写 ====================== """
# 如果在子类中如果有和父类同名的方法，则通过子类实例去调用方法时，
# 会调用子类的方法而不是父类的方法，这个特点我们成为叫做方法的重写（覆盖，override）
class Animal:
    def run(self):
        print('动物会跑~~~')

    def sleep(self):
        print('动物睡觉~~~')

class Dog(Animal):
    def bark(self):
        print('汪汪汪~~~') 

    def run(self):
        print('狗跑~~~~') 

# 当我们调用一个对象的方法时，
#   会优先去当前对象中寻找是否具有该方法，如果有则直接调用
#   如果没有，则去当前对象的父类中寻找，如果父类中有则直接调用父类中的方法，
#   如果没有，则去父类的父类中寻找，以此类推，直到找到 object，如果依然没有找到，则报错

""" ====================== 多重继承 ====================== """
# 在 Python 中是支持多重继承的，也就是我们可以为一个类同时指定多个父类
#   可以在类名的 () 后边添加多个类，来实现多重继承
#   多重继承，会使子类同时拥有多个父类，并且会获取到所有父类中的方法
# 在开发中没有特殊的情况，应该尽量避免使用多重继承，因为多重继承会让我们的代码过于复杂
# 如果多个父类中有同名的方法，则会现在第一个父类中寻找，然后找第二个，然后找第三个。。。
# 前边父类的方法会覆盖后边父类的方法

# 类名.__bases__ 这个属性可以用来获取当前类的所有父类 
print(Person.__bases__) # (<class 'object'>,)
```

##### 多态

```python
# 对于 say_hello() 这个函数来说，只要对象中含有 name 属性，它就可以作为参数传递
# 这个函数并不会考虑对象的类型，只要有 name 属性即可
def say_hello(obj):
    print('你好 %s' % obj.name)
```

##### 模块

> 在 Python 中一个 .py 文件就是一个模块，要想创建模块，实际上就是创建一个 Python 文件
>
> 注意：模块名要符合标识符的规范

```python
# 在一个模块中引入外部模块
# 1.import 模块名 （模块名，就是 Python 文件的名字，注意不要 .py）
#   import 模块名 as 模块别名
#   - 可以引入同一个模块多次，但是模块的实例只会创建一个
#   - import 可以在程序的任意位置调用，但是一般情况下，import 语句都会统一写在程序的开头
#   - 在每一个模块内部都有一个 __name__ 属性，通过这个属性可以获取到模块的名字
#   - __name__ 属性值为 __main__ 的模块是主模块，一个程序中只会有一个主模块
#       主模块就是我们直接通过 Python 执行的模块
# 访问模块中的变量：模块名.变量名
import sys
print(sys.argv)

# 2.from 模块名 import 变量1,变量2,.... (推荐使用)
#	from 模块名 import 变量 as 别名
from m import test
test() # 不用加模块名
```

##### 包

```python
# 包也是一个模块
# 当我们模块中代码过多时，或者一个模块需要被分解为多个模块时，这时就需要使用到包
# 普通的模块就是一个py文件，而包是一个文件夹
# 包中必须要一个一个 __init__.py 这个文件，这个文件中可以包含有包中的主要内容

# __pycache__ 是模块的缓存文件
# .py 代码在执行前，需要被解析器先转换为机器码，然后再执行
#   所以我们在使用模块（包）时，也需要将模块的代码先转换为机器码然后再交由计算机执行
#   而为了提高程序运行的性能，Python 会在编译过一次以后，将代码保存到一个缓存文件中
#   这样在下次加载这个模块（包）时，就可以不再重新编译而是直接加载缓存中编译好的代码即可
```

##### 标准库

```python
""" =================== sys 模块 =================== """
# 1.sys.argv
# 获取执行代码时，命令行中所包含的参数
# 该属性是一个列表，列表中保存了当前命令的所有参数

# 2.sys.modules
# 获取当前程序中引入的所有模块
# modules 是一个字典，字典的 key 是模块的名字，value 是模块对象

# 3.sys.path
# 它是一个列表，列表中保存的是模块的搜索路径

# 4.sys.platform 表示当前 Python 运行的平台

# 5.sys.exit() 退出程序

""" =================== os 模块 =================== """
# 1.os.environ
# 通过这个属性可以获取到系统的环境变量

# 2.os.system(command) 执行操作系统上的命令
os.system('notepad') # 打开记事本

""" =================== pprint 模块 =================== """
# pprint.pprint() 对对象做简单的格式化并打印
# def pprint(object, stream=None, indent=1, width=80, depth=None, *, compact=False)
```



## 文件

##### 打开

> 使用 open() 函数来打开一个文件

```python
# open(file, mode='r', buffering=-1, encoding_=None, errors=None, newline=None, closefd=True, opener=None)
# 参数：
#   file 要打开的文件的名字（路径）
#	mode 打开方式
# 		r 表示只读的
# 			rt 读取文本文件（默认值）
# 			rb 读取二进制文件
# 		w 表示是可写的，使用 w 来写入文件时，如果文件不存在会创建文件，如果文件存在则会截断文件
#   	  截断文件指删除原来文件中的所有内容
# 		a 表示追加内容，如果文件不存在会创建文件，如果文件存在则会向文件中追加内容
# 		x 用来新建文件，如果文件不存在则创建，存在则报错
# 		+ 为操作符增加功能
#   		r+ 即可读又可写，文件不存在会报错
#   		w+
#   		a+
# 返回值：
#   返回一个对象，这个对象就代表了当前打开的文件
open('test.txt')

# with open(file_name) as file_obj : (推荐使用)
# 在 with 语句中可以直接使用 file_obj 来做文件操作
# 此时这个文件只能在 with 中使用，一旦 with 结束则文件会自动 close()
```

##### 关闭

> 调用 close() 方法来关闭文件
>
> 对于 with open 语句不用手动关闭

```python
file_obj = open(file_name) # 打开文件
file_obj.close()		   # 关闭文件
```

##### 读取

> def read(self, n: int = -1)，n 表示每次读取的字符数
>
> def readline(self, limit: int = -1)，读取一行
>
> def readlines(self, hint: int = -1)，一行一行地读取内容，会一次性将读取到的内容封装到一个列表中返回

```python
# open() 默认的编码为 None，需手动指定编码格式
with open(file_name, encoding='utf-8') as file_obj:
    # 如果直接调用 read()，它会将文本文件的所有内容全部都读取出来
    #   如果要读取的文件较大的话，会一次性将文件的内容加载到内存中，容易导致内存泄漏
    #   所以对于较大的文件，不要直接调用 read()，而应指定大小
    content = file_obj.read(6)
    print(content)
    
# <- seek() 修改当前读取的位置 ->
# def seek(self, offset: int, whence: int = 0)
# 	- offset 是要切换到的位置(偏移量)
# 	- whence 计算位置方式
# 		0 从头计算，默认值
# 		1 从当前位置计算
# 		2 从最后位置开始计算

# <- tell() 查看当前读取的位置 ->

```

##### 写入

> 使用 write() 向文件中写入内容，会返回写入字符的个数

```python
with open(file_name, 'w', encoding='utf-8') as file_obj:
    file_obj.write('aaa\n')
```

##### 其它操作

```python
# 1.os.listdir(path='.') 获取指定目录的目录结构，path 是一个路径，返回一个列表
# 2.os.getcwd() 获取当前所在的目录
# 3.os.chdir(path) 切换当前所在的目录 作用相当于 cd
# 4.os.mkdir(path) 创建目录
# 5.os.rmdir(path) 删除目录
# 6.os.remove(path) 删除文件
# 7.os.rename(src, dst) 重命名或移动文件
```

