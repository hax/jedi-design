# Version 1.0.0 (2013/1/18)

Emotion, yet peace.
Ignorance, yet knowledge.
Passion, yet serenity.
Chaos, yet harmony.
Death, yet the Force.
— Jedi Code

# 目录

- [原理](#a0)
    - [何谓表现层（Presentational Layer）](#a0-0)
    - [MVC架构与视图（View）](#a0-1)
    - [什么是模板（Template）](#a0-2)
    - [为什么要设计 Jedi](#a0-3)
    - [Jedi 的未来](#a0-4)
- [源文本](#a1)
    - [字符（Character）和编码（Encoding）](#a1-0)
    - [行（Newline）、空白（Whitespace）和缩进（Indent）](#a1-1)
    - [越位规则（Offside-rule）](#a1-2)
    - [层级结构（Hierarchies）](#a1-3)
- [HTML/XML 构造](#a2)
    - [元素（Element）](#a2-0)
    - [属性（Attribute）](#a2-1)
    - [文本（Text）](#a2-2)
    - [注释（Comment）](#a2-3)
    - [特殊节点](#a2-4)
        - [Document](#a2-4-0)
        - [DocType](#a2-4-1)
        - [PI](#a2-4-2)
        - [CDATA](#a2-4-3)
        - [PCDATA](#a2-4-4)
        - [Entity](#a2-4-5)
        - [IE条件注释](#a2-4-6)
    - [紧凑语法](#a2-5)
        - [E > E](#a2-5-0)
        - [E @attr1 @attr2](#a2-5-1)
        - [E ! CDATA](#a2-5-2)
- [逻辑代码](#a3)
    - [代码抑制（Suppress）和注入（Inject）](#a3-0)
    - [指令（Instruction）](#a3-1)
    - [变量（Variable）和作用域（Scope）](#a3-2)
    - [外部函数和类（External）](#a3-3)
    - [数据绑定（Data Binding）](#a3-4)
    - [条件指令（Conditionals）](#a3-5)
    - [迭代指令（Iteration）](#a3-6)
    - [匹配（Match）](#a3-7)
- [抽象和复用机制](#a4)
    - [元素模板（Element Template）](#a4-0)
    - [内置元素模板（Built-in Templates）](#a4-1)
        - [ol、ul 和 dl](#a4-1-0)
        - [a](#a4-1-1)
        - [script](#a4-1-2)
    - [文档段（Fragment）和模板继承](#a4-2)
- [表达式](#a5)
    - [值和类型](#a5-0)
        - [文字（String）](#a5-0-0)
        - [数字（Number）](#a5-0-1)
        - [逻辑值（Boolean）](#a5-0-2)
        - [时间（Date）](#a5-0-3)
        - [序列（Sequence）](#a5-0-4)
        - [元组（Tuple）](#a5-0-5)
        - [记录（Record）](#a5-0-6)
    - [运算符（Operator）](#a5-1)
    - [函数和方法调用](#a5-2)
    - [属性访问](#a5-3)
    - [数组访问](#a5-4)
    - [解构和匹配](#a5-5)

<a name="a2"/>
# HTML/XML 构造
<a name="a2-0"/>
## 元素（Element）
(1)一个单独的单词就是一个标签: 
```jade
html
```

会被解析为：
```html
<html></html>
```

(2)包涵ID的标签:
```jade
div#container
```

会被解析为：
```html
<div id="container"></div>
```

(3)包涵class的标签：
```jade
div.user-details
```

会被解析为：
```html
<div class="user-details"></div>
```

(4)同时包含class和id:
```jade
div.bar.baz#foo
```

会被解析为：
```html
<div id="foo" class="bar baz"></div>
```

注意： 
class（.xxx）必须写在id（#xxx）的前面，class可以有多个，id只能有一个。 
不能在这里插值。

<a name="a2-1"/>
## 属性（Attribute）
(1)标签的属性可以放置到它下一级子block的任何位置：
```jade
div
	@name1="example"
	'12345
	@name2="text"
```

会被解析为：
```html
<div name1="example" name2="text">
	12345
</div>
```

(2)属性可以插入表达式，插入表达式时要使用双引号。
```jade
a @href="{url}"
	'button
```

会被解析为：
```php
<a href="<?= $data->url ?>">
	button
</a>
```
<a name="a2-2" />
## 文本（Text）
(1)写在单引号 ' 和双引号 " 后面的内容会作为文本输出：

```
'hello!
"world!
```

会被解析为：
```
hello!world!
```

(2)大段文本可以写在子block中：
```
'
	| foo bar baz
	| rawr rawr
	| super cool
	| go jade go
```

会被解析为
```
| foo bar baz | rawr rawr | super cool | go jade go
```

(3)一个单独的'代表一个空格：
```
'
'hello
```

会被解析为：
```
 hello
```

(4)文本开头处的空格会被忽略
```
'           hello
'           world
```

会被解析为：
```
hello world
```

(5)单引号开头的文本写在同一行时，可以在末尾加引号 ' ，末尾的引号 ' 不会被输出。
```
'hello world'
```

会被解析为：
```
hello world
```

(6)以双引号开头的文本可以插入[表达式](#a2-1)
```
"12345 {text}
```

会被解析为：
```
12345 <?= htmlspecialchars($data->text) ?>
```

(7)需要输出 `{}`时，可以用‘\’转义掉它。

```
"12345 \{something}
```

会被解析为：
```
12345 {something}
```

(8)不希望把变量的内容转义掉时，可以使用':unsafe'关键字，强制输出变量：
```
:unsafe adsenseScript
```

会被解析为：
```
<?= adsenseScript ?>
```
<a name="a2-3" />
## 注释（Comment）
(1)用'!'可以把文本注释在HTML页面中：
```
!hello world
```

会被解释为：
```html
<!--hello world-->
```

(2)用'!'注释整段：
```
!
	hello
	world
```

会被解释为：
```
<!--hello world-->
```

(3)JS代码可以用'!'来写。
```
script
	!
		_defer.push('/follow.js');
		alert('hello world');
```

会被解释为：
```
<script>
	_defer.push('/follow.js');
	alert('hello world');
</script>
```

注意：使用'!'写JS代码时，JS中不能插入[表达式](#a2-1)。

(4)若要在JS中插入[表达式](#a2-1)，可以用双引号 " 定义好[变量]，然后在后面使用。
```
script
	"
		var categoryEnglishName = '{category.id}';
	!
		alert(categoryEnglishName);
```

会被解释为：
```
<script>
	var categoryEnglishName = '<?= category.id ?>';
	alert(categoryEnglishName);
</script>
```

## 特殊节点
### Document
### DocType
### PI
### CDATA
### PCDATA
### Entity
### IE条件注释
<a name="a2-5" />
## 紧凑语法
<a name="a2-5-0" />
### E > E
```jade
td > div.linkstd
```

会被解析为：
```html
<td><div class="linkstd"></div></td>
```

<a name="a2-5-1" />
### E @attr1 @attr2
```jade
a @href="www.baixing.com"
	'button
```

会被解析为：
```html
<a href="www.baixing.com">
	button
</a>
```

<a name="a2-5-2" />
### E ! CDATA
```jade
div.text '12345
```

会被解析为：
```html
<div class="text">12345</div>
```
<a name="a3" />
# 逻辑代码
<a name="a3-0" />
## 代码抑制（Suppress）和注入（Inject）
(1)使用'//'可以注释掉一行代码，但是'//'必须写在行首。
```
// just some paragraphs
p 'foo
p 'bar
```

会被解析为：
```
<?php
// just some paragraphs
?>
<p>foo</p>
<p>bar</p>
```

(2)使用字符 '--' 可以简单的注释掉整个block：
```
--div.test
	'example
	span
		'12345
```

会被解析为:
```
<?php
/*
 *div.test
 *	'example
 *	span
 *		'12345
 */
?>
```

指令（Instruction）
变量（Variable）和作用域（Scope）
外部函数和类（External）
数据绑定（Data Binding）
条件指令（Conditionals）
迭代指令（Iteration）
匹配（Match）
抽象和复用机制
元素模板（Element Template）
内置元素模板（Built-in Templates）
ol、ul 和 dl
a
script
文档段（Fragment）和模板继承
表达式
值和类型
文字（String）
数字（Number）
逻辑值（Boolean）
时间（Date）
序列（Sequence）
元组（Tuple）
记录（Record）
运算符（Operator）
函数和方法调用
属性访问
数组访问
解构和匹配