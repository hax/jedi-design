# 目录
- [语法](#a1)
    - [换行符](#a1-0)
    - [标签](#a1-1)
    - [文本](#a1-2)
    - [注释](#a1-3)
    - [关键字](#a1-4)
        - [循环](#a1-4-0)
        - [条件判断](#a1-4-1)
        - [赋值](#a1-4-2)
        - [关键字后置](#a1-4-3)
        - [其他关键字](#a1-4-4)
    - [注入](#a1-5)
    - [模板继承](#a1-6)
    - [闭包](#a1-7)
    - [外部函数与静态方法](#a1-8)

- [词法](#a2)
    - [变量](#a2-0)
    - [表达式](#a2-1)

<a name="a1"/>
# 语法

<a name="a1-0"/>
## 换行符

**CRLF(\r\n)** 和 **CR(\r)** 会被统一解析为 **LF(\n)**。

<a name="a1-1"/>
## 标签

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

(5)标签后面直接跟‘=’,可以直接输出一个[表达式](#a2-1)：
```jade
span = 12345 + 54321
```

会被解析为：
```html
<span>66666</span>
```

(6)在同一行写子标签
```jade
td > div.linkstd
```

会被解析为：
```html
<td><div class="linkstd"></div></td>
```

注意：‘>‘后面只能跟子标签。

(7)标签后面直接跟随文本：
```jade
div.text '12345
```

会被解析为：
```html
<div class="text">12345</div>
```

(8)标签的属性：

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

<a name="a6-3"/>

(9)标签的属性可以插入[表达式](#a2-1)，插入[表达式](#a2-1)时要使用双引号。
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

(8)标签的属性可以后置到它下一级子block的任何位置：
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
<a name="a1-2"/>
## 文本

写在单引号 ' 和双引号 " 后面的内容会作为文本输出：

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

<a name="unsafe">
(8)不希望把[变量](#a2-0)的内容转义掉时，可以使用':unsafe'关键字，强制输出[变量](#a2-0)：
```
:unsafe adsenseScript
```

会被解析为：
```
<?= adsenseScript ?>
```

<a name="a1-3"/>
## 注释

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

(3)用'!'可以把文本注释在HTML页面中：
```
!hello world
```

会被解释为：
```html
<!--hello world-->
```

(4)用'!'注释整段：
```
!
	hello
	world
```

会被解释为：
```
<!--hello world-->
```

(5)JS代码可以用'!'来写。
```
script
	!
		_defer.push('/follow.js');
		alert('bigsheep');
```

会被解释为：
```
<script>
	_defer.push('/follow.js');
	alert('bigsheep');
</script>
```

注意：使用'!'写JS代码时，JS中不能插入[表达式](#a2-1)。

(6)若要在JS中插入[表达式](#a2-1)，可以用双引号 " 定义好[变量]，然后在后面使用。
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

<a name="a1-4"/>
## 关键字
<a name="a1-4-0"/>
### 循环
(1)':for' 关键字  
```
:for ad in ads
	"{ad}
```

会被解析为：
```shell
<?php
foreach ($data->ads as $ad)  { 
	echo htmlspecialchars($ad);
}
?>
```

(2)key，value形式  
```
:for (key, ad) in ads
	"{key} {ad}
```

会被解析为：
```shell
<?php
foreach ($data->ads as $key => $ad)  { 
	echo htmlspecialchars($key), htmlspecialchars(' '), htmlspecialchars($ad);
}
?>
```

(3)多重嵌套的foreach循环：
```
:for x in list1, y in list2
	"{x}, {y}
```

会被解析为：
```
foreach ($data->list1 as $x) {
	foreach ($data->list2 as $y) {
		echo htmlspecialchars($x), htmlspecialchars(', '), htmlspecialchars($y);
	}
}
```

(4)循环控制[变量](#a2-0)可以写任何[表达式](#a2-1)：
```
:for x in [0..3]
	"{x}
```

会被解析为：
```
foreach ([0..3] as $x) {
	echo htmlspecialchars($x);
}
```
<a name="a1-4-1"/>
### 条件判断

(1):if 关键字
```shell
:if a > b
	'hello
```

会被解析为:
```shell
 <?php
if ($data->a > $data->b) {
	echo 'hello';
}
?>
```

(2):else 与 :else if 关键字： 
```shell
:if a > b
	'hello
:else if a < b
	'world
:else
	'earth
```

会被解析为:
```shell
<?php
if ($data->a > $data->b) {
	echo 'hello';
} elseif ($data->a < $data->b) {
	echo 'world';
} else {
	echo 'earth';
}
?>
```

<a name="a1-4-2"/>
### 赋值

（1）:let关键字
```shell
:let x = 'hello world'
	"{x}
```

会被解析为：
```shell
<?php
call_user_func(function($x) {
	echo htmlspecialchars($x);
}, 'hello world');
?>
```

(2)使用:let赋值时，被赋值的[变量](#a2-0)只在它的子block中有效：
```shell
”{x}
:let x = 'hello world'
	"{x}
"{x}
```

会被解析为：
```shell
echo htmlspecialchars($data->x);
<?php
call_user_func(function($x) {
	echo htmlspecialchars($x);
}, 'hello world');
?>
echo htmlspecialchars($data->x);
```

<a name="a1-4-3"/>
### 关键字后置
关键字  if, for, let, else 可以写在标签后面
```
div.content if a > b
```

```
<?php if ($data->a > $data->b) { ?>
	<div class="content"></div>
<?php } ?>
```

<a name="a1-4-4"/>
### 其他关键字：
[:unsafe](#unsafe)
[:import](#a1-6)
[:external](#a1-8)

<a name="a1-5"/>
## 注入
(1)使用字符 '-' 可以向页面中注入PHP代码。  
```shell
-$varb = 'bigsheep';
```

会被解析为:
```shell
<?php
$varb = 'bigsheep';
?>
```

(2)行末的';'可以省略，Jedi会自动添加分号。
```shell
-$varb = 'bigsheep'
```

会被解析为:
```shell
<?php
$varb = 'bigsheep';
?>
```

(3)注入时注意末尾的';'，Jedi的自动添加可能会带来Bug。
```shell
-for ($i = 0; $i < 10; $i++) {
-$varb = 'bigsheep';
-}
```

会被解析为:
```shell
<?php
for ($i = 0; $i < 10; $i++) {;
$varb = 'bigsheep';
};
?>
```

(4)'-'只会影响本行内容，它的子block会继续按照Jedi语法解析，并会自动添加大括号'{  }'。
```
-if ($data->x == 'hello world')
	"{x}
```

会被解析为：
```
if ($data->x == 'hello world')
{
	echo htmlspecialchars(($data->x));
}
```

<a name="a1-6"/>
## 模板继承
Jedi支持简单的模板继承。首先我们建立一个模板  layout.jedi:
```shell
//layout.jedi是一个模板
head
	'我是head
#body
	// '#'代表在body标签上建立了钩子
	'我是body
```
然后使用:import关键字引用这个模板。
Bigsheep.jedi:
```shell
:Import  layout
	#body::brfore
		//Bigsheep.jedi引用了模板layout.jedi
		//在body的内容前增加内容
		'我是Bigsheep
	#body			
		//替换body的内容
		'我也是Bigsheep
	#body::after		
		//在body的内容后增加内容
		'我还是Bigsheep
```

上面的Bigsheep.jedi会被翻译为
```shell
//layout.jedi是一个模板
head
	'我是head
body
	//Bigsheep.jedi引用了模板layout.jedi
	//在body的内容前增加内容
	'我是Bigsheep
	//替换body的内容
	'我也是Bigsheep
	//在body的内容后增加内容
	'我还是Bigsheep
```
注意：目前Jedi的模板只能继承一层，不支持诸如：B继承A，C再继承B。但Jedi支持继承多个模板。

<a name="a1-7"/>
## 闭包
(1)Jedi中的闭包类似于定义一个PHP函数  
```shell
:: time.friendly ()
	'12345
time.friendly = ''
```

会被解析为：
```
<?php
call_user_func(function ($context) {
	echo '<time class="friendly">';
	echo '12345';
	echo '</time>';
}, '');
?>
```

(2)引用闭包时，可以传参数进去。闭包中通过星号 '*' 使用参数。
```shell
:: time.friendly ()
	"{*}
time.friendly =  '12345'
```


会被解析为：
```shell
<?php call_user_func(function ($context) { ?>
	echo '<time class="friendly">';
	echo htmlspecialchars($context);
	echo '</time>';
<?php }, '12345');?>
```

(3)闭包中的参数可以是简单类型的[变量](#a2-0)，也可以是一个数组或者一个实例。
```shell
:: time.friendly ()
	"{*[0]}
	"{*[1]}
time.friendly =  ['12345', '23456']
```

会被解析为：
```shell
<?php call_user_func(function ($context) { ?>
	echo '<time class="friendly">';
	echo htmlspecialchars($context[0]);
	echo htmlspecialchars($context[1]);
	echo '</time>';
<?php }, ['12345', '23456']);?>
```

<a name="a1-8"/>
## 外部函数与静态方法

(1)使用php函数  
```shell
:external is_array
:if is_array(a)
	'Bigsheep
```

会被解析为：
```shell
<?php
if (is_array($data->a)) {
	echo 'Bigsheep';
}
?>
```

(2)引用包涵静态方法的PHP类  
Jedi:
```shell
:external Category
:if Category.exist(categoryName)
	'Bigsheep
```

会被解析为：
```shell
<?php
if (Category::exist($data->categoryName)) {
	echo 'Bigsheep';
}
?>
```

注意： external必须写在文件头部第一行。

<a name="a2"/>
# 词法
<a name="a2-0"/>
## 变量
(1)Jedi变量由字母、数字和'$'组成，不能以数字开头。
(2)变量默认被翻译为$data的属性，如'x'会被翻译为'$data->x'。
(3)由:let和:for产生的临时变量不会被翻译为$data的属性。
      如:let x='12345' 中， 变量 'x' 会被翻译为 '$x' 。
(4)'$'会被当做一般字符进行翻译，如：
      '$x'  会被翻译为   '$data->$x'

<a name="a2-1"/>
## 表达式
(1)运算符优先级：
越往下优先级越高，同一行的运算符优先级一致。
"->" , "<-"
"||" , "&&"
"===" , "!==" , "==" , "!=" , "<=>" , ">=" , "<=" , ">" , "<"
"<<<" , ">>>" , "<<" , ">>"
"+" , "-"
"×" , "÷" , "mul" , "div" , "mod"
'!' , "not"

(2)if then else
Jedi用if then else 替代 ? : 
```
"{if x then '12345' else '54321'}
```
会被解析为
```
<?= $x ? '12345' : '54321' ?>
```

(3)实例变量与实例方法用 点 '.' 连接。
```
"{ad.categoryEnglishName}
``` 

会被解析为
```
<?= $ad->categoryEnglishName ?>
```

```
"{ad.user()}
``` 

会被解析为
```
<?= $ad->user() ?>
```