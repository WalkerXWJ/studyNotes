# javascript 学习笔记

《javaScript权威指南 第6版》 
2024年7月做的笔记，这里面的知识有些陈旧；和现用的javascript有区别；书籍内容基于2009年12月ECMA大会通过的ECMAScirpt 5；至2024年ECMAScript标准已经发布至第15版了。

## 1、词法结构

        javascript程序是使用Unicode字符集编写的，支持地球上几乎所有在用语言。

> javascript严格区分大小写，关键字、变量、函数名、标识符（identifier）都必须采用一致的大小写形式。

### 1-1字符集

#### 1-1-1 空格、换行符、格式控制符

| 序号  | 分类      | 中文名称       | Unicode编码 |
|:---:|:-------:|:----------:|:---------:|
| 1   | 表示空格的字符 | 普通空格符      | `\u0020`  |
| 2   |         | 水平制表符      | `\u0009`  |
| 3   |         | 垂直制表符      | `\u000B`  |
| 4   |         | 换页符        | `\u000C`  |
| 5   |         | 不中断空白      | `\u00A0`  |
| 6   |         | 字节序标记（同14） | `\uFEFF`  |
| 7   | 行结束符    | 换行符        | `\u000A`  |
| 8   |         | 回车符        | `\u000D`  |
| 9   |         | 行分割符       | `\u2028`  |
| 10  |         | 段分割符       | `\u2029`  |
| 11  | 格式控制字符  | 从右至左书写标记   | `\u200F`  |
| 12  |         | 从左至右书写标记   | `\u200E`  |
| 13  |         | 零宽连接符      | `\u200D`  |
| 14  |         | 零宽非连接符（同6） | `\uFEFF`  |

Unicode内码转义写法：前缀`\u` + `4个16进制数`，如：`\u000E`

这种转义写法，可以用在字符串直接量、正则表达式直接量、标识符中（关键字除外）。转义写法写在注释中，会被js忽略，不会被解析成对应的Unicode字符。

```javascript
// 字符 é Unicode转义写法为 \u00E9
"café"=="caf\u00E9"
true

"é"=="\u00E9"
true
```

#### 1-1-2 注释

```javascript
// 单行注释，符号之后的内容被当作注释忽略掉


/* 符号中间的内容会被当作注释忽略，这种注释允许多行书写，但不能有嵌套注释 */

/**
* 多行注释
* 多行注释
*/
```

### 1-2 直接量

直接量：指程序中直接数据值。

```javascript
1    //数字
1.2    //小数
"hello js"    //字符串文本
'hello word'    //另一个字符串
true    //布尔值
false    //另一个布尔值
/javascript/gi    //正则表达式直接量（用作模式匹配）
null    //空
{x:1,y:2}    //对象
[1,2,3,4,5]    //数组
```

### 1-3  标识和保留字

标识符：就是一个名字，用户变量和函数命名或着用于某些循环语句的跳转位置的标记。

标识符命名规则：

> 使用字母、数字、下划线、$符组成

> 不能使用数字开头

```javascript
// 一些合法的标识符
i
my_variable_name
_dummy
$str
```

我们通常使用ASCII字母和数字来书写标识符；担javascript允许标识符出现Unicode字符全集中的字母和数字，从技术上讲ECMAScrip标准也允许在标识符的首字母后出现Unicode字符集中Mn类、Mc类、Pc类。

- Mn类：表示基字符的修改中出现的非间距字符；
- Mc类：表示基字符的修改中影响了基字符标志位的宽度的间距字符；
- Pc类：指连接两个符号的连接符或标点符号。

```javascript
// 如下都是合法的标识符号
var aí = "hello word";
var π = 3.1415926
```

保留字（关键字）： javascript将一部分标识符号拿出来作为自己所用，用户不能在程序中将这些关键字用作标识符。

```javascript
//正在使用的关键字
break        delete        function        return        typeof
case        do            if                switch        var
catch        else        instanceof        throw        while
debugger    finally        new                true        with
default        for            null            try        

//ECMAScript 5未使用但保留的关键字
class        const        enum            export        extends
import        super

//下面这些关键字在普通的javascript代码中是合法的，在严格模式下是保留字
implements	let		private		public		yield
interface	package	protected	static
```

### 1-4 可选的分号
javascript使用分号`;`分割语句，这对增强代码的可读性和整洁性十分重要。如果一条语句独占一行，通常可以省了语句结尾的分号，右花括号`}`前的分号也可以省略。

两种编程风格：
- 正常使用分号`;`分割所有语句
- 只在必须使用分割`;`时才使用分割符

如下时一些示例：
```javascript
// 如下代码可以省略第一个分号
var a = 1;
var b = 2;
```

## 2、类型、值、变量

## 3、表达式、运算符

## 4、语句

## 5、对象

## 6、数组

## 7、函数

## 8、类、模块

## 9、正则表达式的模式匹配

## 10、javascript的子集和扩展

## 11、服务器端javascript
