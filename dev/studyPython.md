# python编程-学习笔记

[在线资料](https://www.ituring.com.cn/book/2784)

## 1 环境：

笔记代码使用环境：python3.9.13

<mark>终端python常用的命令</mark>

- 输出python版本

`# python --version`

python程序源文件以`.py`结尾

<mark>文件头部注释：</mark>

python文件的编码声明：`-*-`只是为美观，没有含义

```python
# -*-encoding:utf-8-*-
```

### 1.1  Sublime text 编辑器

    Sublime text 免费的文本编辑器，支持多系统，几乎可以在编辑器中执行所有程序。

    Sublime text 配置，使Sublime使用正确版本的python环境，如果执行命令python时启动的事python3，则无需配置；配置的方法：》Build System  〉New Build System ，新建一个文件，删除文件中的所有内容，并输入一下内容：

```python
{
    "cmd" : ["python3", "-u", "$file"],
}
```

    将文件保存到Sublime默认打开的文件夹中，命名为`Python3.subliem-build`.

    python程序允许的三种方式：

- 快捷键：` command +  B`

- 菜单选择：》Tools 〉Build  

- 菜单选择：》Tools 〉Build System 》Python3

### 2.1 格式

- 缩进：所有的缩进统一为4个空格

- 代码行：建议代码行长不超过79个字符

- 空行：可以用来分割不同功能的代码块

- 注释行：建议行长不超过72个字符

## 2 变量和简单数据类型

### 2.1变量

变量：内存中的一块区域，指向特定的值。

#### 2.1.1 变量声明

```python
message = "this is message"     #声明变量
print(message)         #打印变量
```

#### 2.1.2 变量命名规范

1. 变量名只能包含数字、字母、下划线，不能以数字开头；

2. 变量不能包含空格；

3. 变量命名不能是用python关键字、函数名；

4. 变量命名应简单，且见名知意。

### 2.2 字符串

字符串：一系列字符，在python中指被单引号或双引号括起来的字符。

```python
messages1 = "this is a String message"
messages2 = 'this is a String meesage'
```

#### 2.2.1 使用方法修改字符串的大小写

1、字符串首字母大写，使用`title()`函数处理

```python
name = "xiao ming"    #变量name指向字符串"xiao ming"
print(name.title())    #对name变量进行首字母大写的处理，在打印处理后的结果
```

输出：`Xiao Ming`

2、字符串全部大写，使用`upper()`函数操作

```python
name = "xiao ming"
print(name.upper())    #对name变量进行字母全大写处理，在打印处理后的结果
```

输出：`XIAO MING`

3、字符串全小写，使用`lower()`函数操作

```python
name = "XIAO MING"
print(name.lower())    #对name变量进行字母全小写处理，在打印处理后的结果
```

输出：`xiao ming`

#### 2.2.2 字符串中使用变量

`f字符串`f是format的简写，python中<mark>通过将花括号中的变量替换成其值来设置格式化字符串</mark>；

`f字符串`是python3.6引入的，在使用python3.5或之前的版本则需要使用`format()`函数；

**`f字符串` 格式化处理**

```python
first_name = "xiao" # 声明变量
last_name = "ming"  # 声明变量
full_name = f"{first_name} {last_name}" # 将first_name和last_name通过f字符串进行格式化处理
print(f"Hello, {full_name.title()}")    # full_name通过和title()函数进行首字母大写处理，在通过f字符串进行格式化处理，在通过print()函数进行打印
```

**使用`format()`函数进行字符串格式化处理**

```python
first_name = "xiao"
last_name = "gang"
message = "Hello, {} {}".format(first_name,last_name).title()   #将通过format格式处理后的字符串传进行首字母大写处理 在赋值给变了message
print(message)
```

#### 2.2.3 使用制表符或换行符添加空白

空白：泛指任何非打印字符，如空格，制表符，换行符。

<mark>空格</mark>：` `    <mark>制表符</mark>：`\t`    <mark>换行符</mark>：`\n`

**制表符：**

```python
print("\thello") #\t为制表符
```

输出：

`    hello`

**换行符：**

```python
print("good\ngood") #\n为换行符
```

输出：

`good`

`good`

**组合使用：**

```python
print("Language:\n\tpython\n\tC#\n\tjava")
```

输出：

`Language:`
`    python`
`    C#`
`    java`

#### 2.2.4 删除空白

**1、去掉结尾空白（空格、制表符、换行符等），使用`rstrip()`函数操作   #<mark>右侧剥离</mark>。**

```python
favorite_language = " python\n\t "
print(favorite_language.rstrip()) #使用rstrip()函数去掉结尾的空白，操作不改变原始变量的值
```

输出：` python`末尾的空白被去掉了，右侧的空格被保留。

**2、去掉开头空白，使用`lstrip()`函数操作。#<mark>左侧剥离</mark>**

```python
favorite_language = " \n\tpython "
print(favorite_language.lstrip()) #使用lstrip()函数去掉开头的空白，操作不改变原始变量的值
```

输出：`python `开头的空白被去掉了，左侧的空格被保留。

3、同时去掉开头和结尾的空格，使用`strip()`函数操作

```python
favorite_language = " \n\tpython\n\t "
print(favorite_language.strip()) #使用strip()函数同时去掉开始和结尾的空白
```

输出：`python`

### 2.3 数字

python中的算术运算符：<mark>加</mark>`+`<mark>减</mark>`-`<mark>乘</mark>`*`<mark>除</mark>`/`<mark>乘方</mark>`**`

#### 2.3.1 整数

```python
print(1 + 2)
```

输出：`3`

#### 2.3.2 浮点数

浮点数：python中将带有小数点的数称为浮点数。

浮点数的计算，可能会和预期存在差异；

```python
print(0.2 + 0.1)
print（3 * 0.1）
print(3*0.3)
```

输出：

`0.30000000000000004`

`0.30000000000000004`

`0.8999999999999999`

#### 2.3.3 整数与浮点数

- 任何两个数相除，结果总是浮点数。即使两个数都是整数，且能整除。

- 如果其中一个操作数是浮点数，结果也总是浮点数。

#### 2.3.4 数字中的下划线

数字中的下划线可以给数字分组，使数字易读。python看来10_00和1_000没有区别，打印时不会输出下划线。

```python
print(1_000_000_000)
```

输出：`1000000000`

#### 2.3.5 同时给多个变量赋值

在一行代码中给多个变量赋值，有助于缩短代码提高可读性；

```python
x,y,z = 0,0,0 # 将变量x，y，z都初始化为0
print("{} {} {}".format(x,y,z))
```

输出：`0 0 0`

#### 2.3.6 常量

常量：类似于变量，但其值在程序的整个生命周期内保持不变。

```python
MAX_CONNECTIONS = 500 #python没有内置的常量类型，通常通过全大写视为常量，其值应始终不变
```

### 2.4 注释

注释：在程序使用自然语言添加说明内容，注释的内容会被python解释器忽略。

#### 2.4.1 单行注释

单行注释：使用`#`符号

```python
# 这里是单行注释内容，注释内容python不会解释执行
print("")
```

#### 2.4.2 python之禅

在python终端会话中输入：`import this`,将输出python编程的指导原则。

```python
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.    #美丽总比丑陋好。
Explicit is better than implicit.    #显式总比隐式好
Simple is better than complex.    #简单总比复杂好。
Complex is better than complicated.    #复杂总比更复杂好。
Flat is better than nested.    #平坦比嵌套好。
Sparse is better than dense.    #稀疏比密集好。
Readability counts.    #可读性很重要。
Special cases aren't special enough to break the rules. #特殊情况不足以违反规则。
Although practicality beats purity.    #虽然实用性胜过纯洁。
Errors should never pass silently.    #错误永远不应该默默地过去。
Unless explicitly silenced.    #除非明确沉默。
In the face of ambiguity, refuse the temptation to guess.    #面对模棱两可，拒绝猜测的诱惑。
There should be one-- and preferably only one --obvious way to do it.    #应该有一种——最好只有一种——显而易见的方法。
Although that way may not be obvious at first unless you're Dutch.    #尽管除非您是荷兰人，否则这种方式起初可能并不明显。
Now is better than never.     #现在总比没有好。
Although never is often better than *right* now.    #虽然从来没有比现在*正确*更好。
If the implementation is hard to explain, it's a bad idea.   #如果实现难以解释，这是一个坏主意
If the implementation is easy to explain, it may be a good idea.    #如果实现很容易解释，这可能是一个好主意。
Namespaces are one honking great idea -- let's do more of those!    #命名空间是一个很棒的主意 - 让我们做更多的事情！
```

## 3 列表

### 3.1 列表

列表：由一系列按特定顺序排列的元素组成，适合存储程序运行过程可能变化的数据集。

python中使用<mark>方括号`[]`表示列表</mark>，使用英文的逗号`,`分割元素。

```python
numbers = [1,2,3,4,5,6,7,8,9,0]    #初始化列表并赋值
print(numbers)    #打印列表
```

输出：`[1, 2, 3, 4, 5, 6, 7, 8, 9, 0]`

#### 3.1.1访问列表元素

列表元素通过索引访问，<mark>正序访问索引从0开始</mark>，而不1开始。依次`0`访问第一个元素，`1`访问第二个元素

```python
strs = ["aa","bb","cc","dd","ee","ff"]
print(strs[0].title()) #访问第一个元素，使用title()首字母大写处理后打印
```

输出：`Aa`

超过索引范围访问

```python
strs = ["aa","bb","cc","dd","ee","ff"]
print(strs[6].title())
```

输出：`IndexError: list index out of range`索引错误：列表索引超出范围

<mark>倒序访问列表索引从-1开始</mark>，依次`-1`访问倒数一个元素，`-2`访问倒数第二个元素。。。

```python
strs = ["aa","bb","cc","dd","ee","ff"]
print(strs[-1].title())  #访问倒数第一个元素，使用title()首次大写处理后打印
```

输出：`Ff`

#### 3.1.2 修改列表元素

```python
strs = ["aaa","bbb","ccc"] # 初始化列表并赋值
print(strs)
strs[0] = "zzz" #修改列表strs索引为0的元素值，通过指定索引赋值来修改列表元素
print(strs)
```

输出：

`['aaa', 'bbb', 'ccc']`

`['zzz', 'bbb', 'ccc']` #修改后的列表索引为0的元素发生了改变

#### 3.1.3 附加列表元素

列表中添加元素简单的方法是将元素附加<mark>`append()`</mark>函数到列表,将添加到列表末尾。

```python
strs = ["aaa","bbb","ccc"]
strs.append("ddd") #通过append()函数将元素添加到列表尾部，原列表值改变
print(strs)
```

输出：`['aaa', 'bbb', 'ccc', 'ddd']`

通过`append()`函数动态常见列表

```python
strs = []  #初始话空列表
strs.append("aaa") #通过append()给空列表附加元素，原列表值改变
strs.append("bbb")
strs.append("ccc")
print(strs)
```

输出：`['aaa', 'bbb', 'ccc']`

#### 3.1.4 插入列表元素

使用<mark>`insert(<index>，<value>)`</mark>函数可以在列表任意位置添加元素，原列表值改变。需要指定新元素的索引和值。

```python
strs = ["aaa","bbb","ccc"]
strs.insert(0,"zzz") #通过insert在索引0的位置添加空间，并将值存储在新添加的空间，操作将原有元素都向右移动一个位置
print(strs)
strs.insert(-1,"yyy") #通过insert在索引-1位置添加空间，并将值存储到新添加的共建，原来-1位置的元素右移一位置
print(strs)
```

输出：

`['zzz', 'aaa', 'bbb', 'ccc']`

`['zzz', 'aaa', 'bbb', 'yyy', 'ccc']`

#### 3.1.5 删除列表元素

1、使用<mark>`del`语句</mark>删除列表元素，原列表值改变

```python
strs = ["aaa","bbb","ccc"]
del strs[1] # 使用del 语句删除索引为1位置的元素，原列表值改变
print(strs)
# del strs[2] # 索引超出范围报错：IndexError: list assignment index out of range
```

输出：`['aaa', 'ccc']`

2、使用方法<mark>`pop()`函数</mark>弹出列表末尾元素，术语弹出（pop）源自类比：列表就像一个栈，删除末尾元素相当于弹出栈顶元素。

```python
strs = ["aaa","bbb","ccc"]
values = strs.pop() #通过pop()函数删除末尾元素，并将返回的元素之赋值给变量values，原列表值已改变
print(strs)
print(values)
```

输出：

`['aaa', 'bbb']`

`ccc`

3、使用<mark>`pop(<index>)`函数</mark>弹出任意索引的元素，需要在括号中指定索引。<mark>使用`pop()`弹出元素后，原列表中元素也删除了。</mark>

```python
strs = ["aaa","bbb","ccc"]
values = strs.pop(1) #通过pop()函数删除索引为1的元素，并将返回的元素之赋值给变量values
print(strs)
print(values)
```

输出：

`['aaa', 'ccc']`

`bbb`

4、使用<mark>`remove()`函数</mark>根据元素值删除元素

```python
strs = ["aaa","bbb","bbb","ccc"] #初始化列表
value = "ccc" # remove() 没有返回值，如果后续需要使用元素值，需要赋值给变量存储值
strs.remove(value) #通过remove()删除第一匹配值的元素，
print(strs)
```

输出：`['aaa', 'bbb', 'ccc']`

### 3.2 列表排序

#### 3.2.1  使用`sort()`函数对列表永久排序

- sort -- 种类，把...分类，`sort()`无参数时<mark>按照字母表顺序排序</mark>，函数无返回值

```python
strs = ["bbb","aaa","ddd","ccc"]
strs.sort() #使用sort()函数按照字母表顺序对列表元素排序，将修改原列表的元素顺序
print(strs)
```

输出：`['aaa', 'bbb', 'ccc', 'ddd']`

- <mark>按照字母表相反顺序排序</mark>，需要在`sort()`中传递`reverse = True`参数，函数无返回值

```python
strs = ["bbb","aaa","ddd","ccc"]
strs.sort(reverse = True) #使用sort(reverse = True)函数按照字母表相反顺序对列表元素排序，将修改原列表的元素顺序
print(strs)
```

输出：`['ddd', 'ccc', 'bbb', 'aaa']`

#### 3.2.2 使用`sorted()`函数对列表临时排序

- 使用`sorted(<list>)`函数<mark>按字母表顺序</mark>对列表进行临时排序，函数需要列表参数，函数返回排序后的列表

```python
strs = ["bbb","aaa","ddd","ccc"]
strl = sorted(strs) #使用sorted(list)函数按照字母表顺序对列表临时排序，不改变原列表的排序
print(strl)
print(strs)
```

输出：

`['aaa', 'bbb', 'ccc', 'ddd']`

`['bbb', 'aaa', 'ddd', 'ccc']`

使用`sorted(<list>,reverse = True)`函数<mark>按字母表相反顺序</mark>对列表进行临时排出，函数需要两个参数，函数返回排序后的列表

```python
strs = ["bbb","aaa","ddd","ccc"]
strl = sorted(strs,reverse = True) #使用sorted(list)函数按照字母表相反顺序对列表临时排序，不改变原列表的排序
print(strl)
print(strs)
```

输出：

`['ddd', 'ccc', 'bbb', 'aaa']`

`['bbb', 'aaa', 'ddd', 'ccc']`

#### 3.2.3 使用`reverse()`函数反转列表元素的排序

使用`reverse()`函数反转元素排序，将反转原列表的排序顺序，函数没有返回值。如果要恢复原来的排序可以再次调用`reverse()`函数。

```python
strs = ["bbb","aaa","ddd","ccc"]
strs.reverse() #反转列表排序，原列表排序被修改，函数无返回值
print(strs)
```

输出：

`['ccc', 'ddd', 'aaa', 'bbb']`

#### 3.2.4 使用`len()`函数，获取列表的长度

`len(<list>)`函数需要传递列表参数，返回列表的长度，即列表包含元素的个数。

```python
strs = ["bbb","aaa","ddd","ccc"]
num = len(strs) # 将len()返回值存储到变量num
print(f"列表strs中包含的元素个数为：{num}") # 格式化输出
```

输出：`列表strs中包含的元素个数为：4`

### 3.3 列表遍历

#### 3.3.1 for循环列表遍历

```python
strs = ["bbb","aaa","ddd","ccc"]
for sts in strs:  # 定义for循环
    print(sts)  #python具有严格的缩进要求，for循环打印列表元素，实现遍历
```

输出：

`bbb`
`aaa`
`ddd`
`ccc`

### 3.4 数值列表

`range()`函数可以给起始值和结束值两个参数或只给一个结束值参数或给开始值、结束值、步长三个参数。从指定的第一个值开始，到第二个值结束，不含第二个值。

- 使用`range()`函数打印一系列数

```python
for value in range(1,4):   #使用for循环遍历range
    print(value)
```

输出：

`1`

`2`

`3`

#### 3.4.1 创建数值列表

- 使用`range()`函数和`list()`函数创建列表

  `range(1,4)`生成一组数，list(range(1,4)返回列表

```python
strs = list(range(1,4)) #将创建一个列表 [1, 2, 3]存储到strs
print(strs)
```

输出：`[1, 2, 3]`

- 使用`range()`函数和`list()`函数创建一个具有一定步长的列表

  创建一个包含10以内奇数的列表

```python
strs = list(range(1,10,2)) #range起始值1，结束值10（不含），步长2，使用list()返回列表
print(strs)
```

输出：`[1, 3, 5, 7, 9]`

- 创建一个含有20以内偶数乘方的列表

```python
strs =[] # 声明空列表
for value in range(2,21,2):  #定义for循环
    strs.append(value ** 2)  #通过循环将range数字值做成方计算后，通过append()依次附加到列表结尾
print(strs)
```

输出：`[4, 16, 36, 64, 100, 144, 196, 256, 324, 400]`

#### 3.4.2 数值列表简单统计

`min(<list>)`   返回列表最小值,需要传递列表参数

`max(<list>)`    返回列表最大值,需要传递列表参数

`sum(<list>)`    返回列表值的总和,需要传递列表参数

```python
print(len(strs))
for value in range(2,21,2):  #定义for循环
    strs.append(value ** 2)  #通过循环将range数字值做成方计算后，通过append()附加到列表结尾
print(strs)
print(f"strs列表最小值：{min(strs)}")
print(f"strs列表最大值：{max(strs)}")
print(f"strs列表总和：{sum(strs)}")
```

输出：

`[4, 16, 36, 64, 100, 144, 196, 256, 324, 400]`
`strs列表最小值：4`
`strs列表最大值：400`
`strs列表总和：1540`

#### 3.4.2 列表解析

列表解析：将for循环和创建元素的代码合成一行，并自动附加新元素。

3.4.2中的代码，通过列表解析实现

```python
strs = [value ** 2 for value in range(2,21,2)] #通过列表解析创建列表
print(strs)
```

输出：`[4, 16, 36, 64, 100, 144, 196, 256, 324, 400]`

上述代码解释：

1. `strs` 描述性列表名，列表变量名

2. `value **2` 为指定表达式，用于生成存储到列表的值

3. `for value in range(2,21,2)` for循环，为表达式提供值

4. 表达和for循环使用中括号`[]`括起来

### 3.5 使用列表的一部分

#### 3.5.1 切片定义

从起始索引开始，到结束索引结束，不包含结束索引元素。

```python
numb_list = [value for value in range(1,50,2)] #使用列表解析创建列表
print(numb_list)
numb_slice = numb_list[0:5] #生成索引0-4的数据切片
print(numb_slice)
numb_slice = numb_list[3:6] #生成索引3-5的数据切片
print(numb_slice)
numb_slice = numb_list[:5] #生成索引0-4的数据切片，未指定起始索引默认从0开始
print(numb_slice)
numb_slice = numb_list[15:] #生成索引15-最后一个元素的数据切片，为指定结束索引，默认到列表最后一个元素结束
print(numb_slice)
```

输出：

`[1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 25, 27, 29, 31, 33, 35, 37, 39]`
`[1, 3, 5, 7, 9]`
`[7, 9, 11]`
`[1, 3, 5, 7, 9]`
`[31, 33, 35, 37, 39]`

#### 3.5.2 遍历切片元素

```python
numb_list = [4, 16, 36, 64, 100, 144, 196, 256, 324, 400] #初始化列表
for numb in numb_list[0:4]: # 切片获取索引0-3的列表切片，for循环遍历切片
    print(numb) #利用for循环打印切片元素
```

输出：

`4`
`16`
`36`
`64`

#### 3.5.3 通过切片复制列表

切片通过同时省略起始索引和结束索引，可以获得有和原列表元素一致的切片，<mark>通过切片复制列表将得到两个不同列表</mark>。

```python
numb_list_1 = [1, 3, 5, 7, 9]
numb_list_2 = numb_list_1[:] #获取与原列表一致的切片
print(numb_list_2)
numb_list_1.append("AAA")    #为列元素numb_list_1附加元素"AAA"
numb_list_2.append("BBB")    #为列元素numb_list_2附加元素"BBB"
print(numb_list_1)
print(numb_list_2)
```

输出：通过输出证明列表1和列表2为不同列表，附加的元素对两个列表互不影响

`[1, 3, 5, 7, 9]`
`[1, 3, 5, 7, 9, 'AAA']`
`[1, 3, 5, 7, 9, 'BBB']`

<mark>通过列表直接赋值的方式，不会得到两个不同的列表，而是使两个变量同时指向同一个列表</mark>

```python
numb_list_1 = [1, 3, 5, 7, 9]
numb_list_2 = numb_list_1    #是numb_lsit_2指向numb_list_1
numb_list_1.append("AAA")    #列表1附加的元素和列表附加的元素，都附加在同一个列表中了
numb_list_2.append("BBB")
print(numb_list_1)
print(numb_list_2)
```

输出：

`[1, 3, 5, 7, 9, 'AAA', 'BBB']`
`[1, 3, 5, 7, 9, 'AAA', 'BBB']`

### 3.6 元组

元组：python中不可变的列表，元组中的元素不可修改。

元祖：使用小括号标识，严格的说元组是有英文的逗号标识的，括号使元组看起来更整洁，如果<mark>声明值包含一个元素的元组，必须在元素后面添加逗号</mark>。

如 `numb_tuple = (1,)` `numb_tuple = 1,`

以下元组的定义也是合法的。

```python
numb1_tuple = 1,2,3,    #定义元组
numb2_tuple = 1,2,3     #定义元组
numb3_tuple = (1,2,3,)  #定义元组
```

#### 3.6.1 定义元组

```python
numb_tuple = (1,2,3)    #定义元组
print(numb_tuple)    #打印元组
print(numb_tuple[0])    #获取元组的第一个元素 并打印
print(numb_tuple[1])    #获取元组的第二个元组 并打印
```

输出：

`(1, 2, 3)`
`1`
`2`

元组的元素是不可修改的，下面尝试修改元组的元素。

```python
numb_tuple = (1,2,3)
numb_tuple[0] = 6 #本行尝试修改元组元素的代码将报错
```

以上代码将报错：`TypeError: 'tuple' object does not support item assignment`

                                类型错误：‘元组’对象不支持指派元素

#### 3.6.2 遍历元组元素

```python
numb_tuple = (1,2,3,)  #定义元组
for numb in numb_tuple:    #for循环遍历元组
    print(numb)    #利用for循环打印元组的元组
```

输出：

`1`

`2`

`3`

#### 3.6.3 修改元组变量

虽然元组的元素不可修改，但可以给元组变量重新赋值来修改元组变量。

```python
numb_tuple = (1,2,3,)  #定义元组
print("元组变量重新定义前：")
for numb in numb_tuple: # 元组遍历
    print(numb)
numb_tuple = 1,2,   #重新定义元组
print("元组变量重新定义后")
for numb in numb_tuple: #元组遍历
    print(numb)
```

输出：

`元组变量重新定义前：`
`1`
`2`
`3`
`元组变量重新定义后`
`1`
`2`

## 4 if语句

if语句属于分支控制结构，允许通过条件决定执行的内容。

if语句的核心是一个值True或false的表达式，这种表达式称为条件测试。

### 4.1 if语句定义

```python
strs_list = ["bbb","aaa","ddd","ccc"] # 定义列表
for value in strs_list:    #for循环
    if value == "ddd":    #if条件判断
        print(value.upper())    #满足条件执行的内容，进行全大写处理并打印
    else:    #else语句
        print(value.title())    #不满足条件执行的内容，进行首字母大写并打印
```

输出：

`Bbb`
`Aaa`
`DDD`
`Ccc`

### 4.2 条件测试

#### 4.2.1 相等运算符`==`

相等运算符`==`，比较中如果大小写重要，则直接比较。

```python
value = "AAA"    #定义变量value
print(value == "AAA")    #打印 value和"AAA" 做相等运算后的返回值
print(value == "aaa")
```

输出：

`True`
`False`

相等运算符`==`，比较中大小写不重要，统一小写处理后在比较，小写处理不改变原变量的值。

```python
value = "AAA"    #定义变量
print(value.lower() == "AAA".lower())    #小写处理后做==比较，并打印返回值
print(value.lower() == "aaa")    #小写处理后做==比较，并打印返回值
```

输出：

`True`

`True`

#### 4.2.4 不等运算符!=

不等运算符`!=`,英文感叹号的含义是`不`

```python
value = "AAA"
print(value != "AAA")    #运算符两侧做不等比较，并打印返回值
print(value != "aaa")
print(value.lower() != "AAA".lower()) #做小写处理后，运算两次做不等比较，并打印返回值
print(value.lower() != "aaa")
```

输出：

`False`
`True`
`False`
`False`

#### 4.2.3 大于、小于、大于等于、小于等于

数据值比较

```python
print(1 > 2)    #数值大于比较
print(1 >= 2)   #数值大于等于比较
print(1 < 2)    #数值小于比较
print(1 <= 2)   #数值大于等于比较
print(1 == 2)   #数值相等比较
print(1 != 2)   #数值不等比较
```

输出:

`False`
`False`
`True`
`True`
`False`
`True`

4.2.4 多条件

关键字`and` : 检查两个条件是否都为Ture, 执行操作

关键字`or` : 检查其中一个条件为Ture , 执行操作

```python
print(1<2 and 2<3)    # Ture
print(1>2 and 2<3)    # False
print(1<2 or 2<3)    #True
print(1>2 or 2<3)    #True
```

and多添件判断

| 序号  | 表达式1结果 | 表达式2结果 | 最终结果  |
|:---:| ------ | ------ | ----- |
| 1   | True   | True   | True  |
| 2   | Ture   | False  | False |
| 3   | False  | Ture   | False |
| 4   | False  | False  | False |

or多条件判断

| 序号  | 表达式1结果 | 表达式2结果 | 最终结果  |
|:---:| ------ | ------ | ----- |
| 1   | True   | True   | True  |
| 2   | Ture   | False  | True  |
| 3   | False  | Ture   | True  |
| 4   | False  | False  | False |

#### 4.2.4 检查特定的值是否包含在列表中

需要判断特定的值是否已包含在列表中,使用关键字<mark>`in`</mark>.

```python
strs_list = ["bbb","aaa","ddd","ccc"] #声明列表
st = "aaa"    
if st in strs_list :    # 使用关键字in关键字,判断列表是否包含某个变量
    print(f"{st} 包含在列表中")
else:
    print(f"{st} 不包含在列表中")
```

输出:`aaa 包含在列表中`

#### 4.2.5 检查特定的值是否不包含在列表中

需要判断特定的值是否不包含在列表中,使用关键字<mark>`not in`</mark>.

```python
strs_list = ["bbb","aaa","ddd","ccc"]
st = "aaa"
if st not in strs_list:    #使用not in关键字,判断是否不包含某个变量
    print(f"{st} 不含在列表中")
else:
    print(f"{st} 包含在列表中")
```

#### 4.2.5 布尔表达式

布尔表达式:布尔表达式的结果为`Ture / False`.

### 4.3 if语句

if语句的最基本结构

```python
if conditional_test :
    do somthing
'''
# if关键字
# conditional_test    测试条件/布尔表达式
# :    
'''
```

```python
age = 22
if age > 18 : # 判断age 是否大于18 ，True 则继续
    print("good job") #执行缩进处的函数调用 print()
```

### 4.3 if-else语句

if - else 语句，如果未满足if的条件，则执行else代码块

```python
age = 18
if age > 18 :
    print('good job')
else :
    print('your are a baby')
```
