# 目录
- [第一章  新手入门](#a1)
    - [0 搭建环境](#a1-0)
    - [1 HTML页面](#a1-1)
    - [2 页面逻辑](#a1-2)
    - [3 注入与注释](#a1-3)
    - [4 静态方法与外部方法](#a1-4)
- [第二章  home、listsing与viewAd](#a2)
    - []
    - []
    - []
- [第三章 ]
# 第一章 新手入门

必备技能：HTML，java script， PHP。

本章将介绍怎样用Jedi完成简单的网页。

本指南系入门教程，不会覆盖过多细节。

## 0. 搭建环境

第一步：搭建haojing环境。

第二步：在htdocs/pages/  文件夹下新建jediTest.php：
```shell
<?php
echo (new Template('jediTest'))->render();
?>
```

第三步：在htdocs/view/  文件夹下新建jediTest.jedi：
```shell
‘hello world
```

第四步：打开www.yourname.baixing.cn/p/jediTest.php，出现hello world，环境搭建成功！

## 1. HTML页面

### 一、标签
(1)jedi中，直接写一个单独的单词即代表一个HTML标签。  
Jedi:
```shell
div
```

对应的HTML:
```shell
<div></div>
```

(2)标签的属性写在‘@’后面。  
Jedi:
```shell
div @name="BigSheep" @nick="大绵羊"
```

对应的HTML:
```shell
<div name="BisSheep" nick="大绵羊"></div>
```

(3)标签的class属性可以直接用'.'表示，id属性可以直接用'#'表示。  
Jedi:
```shell
Span.Big#sheep @name="BigSheep"
```

对应的HTML:
```shell
<span class="Big" Id="sheep“ name="BigSheep"></span>
```

### 二、文本

(1)以单引号开头时，写什么输出什么：  
Jedi:
```shell
'12345^&*(
```

对应的HTML:
```shell
12345^&*(
```

(2)以双引号开头时，可以使用'{}'插入PHP变量：  
Jedi：
```shell
"{Bigsheep}
```

对应的PHP:
```shell
<?php
echo htmlspecialchars($data->bigsheep);
?>
```

(3)以！开头的内容为注释掉的文本，会输出到浏览器中，但不会显示出来。  
Jedi:
```shell
! 大绵羊是noooooooooooooob？！
```

对应的HTML:
```shell
<!-- 大绵羊是noooooooooooooob？！ -->
```

### 三、层次关系

1）Jedi由缩进表达层级关系。  
Jedi：
```shell
div
	'12345
```

对应的HTML：
```shell
<div>
	12345
</div>
```

Jedi:
```shell
div
'12345
```

对应的HTML：
```shell
<div></div>
12345
```

(2)Jedi由缩进表达层级关系，例二。  
Jedi:
```shell
div
	span.Big#sheep @name="BigSheep"
		'12345
		//注意:Jedi中必须每条语句一行。
	div.bigsheep
```

对应的PHP:
```shell
<div>
	<span class="Big" Id="sheep" name="BigSheep">
		12345
		<?php //注意:Jedi中必须每条语句一行。?>
	</span>
	<div class="bigsheep"></div>
</div>
```

## 2. 页面逻辑

### 一、循环：
(1)简单foreach  
Jedi:
```shell
:for sheep in sheeps
	"{sheep}
```

对应的PHP:
```shell
<?php
foreach ($data->sheeps as $sheep)  { 
	echo htmlspecialchars($sheep);
}
?>
```

(2)key，value形式的foreach  
Jedi:
```shell
:for (key, value) in sheeps
	"{key} {value}
```

对应的PHP：
```shell
<?php
foreach ($data->sheeps as $key => $value)  { 
	echo htmlspecialchars($key), htmlspecialchars(' '), htmlspecialchars($value);
}
?>
```

### 二、条件判断

(1)裸if  
Jedi:
```shell
:if a > b
	'bigsheep
```

对应的PHP:
```shell
 <?php
if ($data->a > $data->b) {
	echo 'bigsheep';
}
?>
```

(2)if + else + elseif  
Jedi:
```shell
:if a > b
	'huge sheep
:else if a < b
	'normal sheep
:else
	'big sheep
```

对应的PHP:
```shell
<?php
if ($data->a > $data->b) {
	echo 'huge sheep';
} elseif ($data->a < $data->b) {
	echo 'normal sheep';
} else {
	echo 'big sheep';
}
?>
```

### 三、赋值

（1）使用:let关键字对临时变量进行赋值  
Jedi：
```shell
:let bigsheep = ‘杨敏达’
		"{bigsheep}
```

对应的PHP：
```shell
<?php
$bigsheep = ‘杨敏达’;
echo htmlspecialchars($bigsheep);
?>
```

（2）使用:let赋值时，只对当前block生效  
Jedi(设$data->a的值为字符串’1234’):
```shell
"{a}
:let a = ‘4321’
		"{a}
"{a}
```

最后输出到页面上的结果：
```shell
1234
4321
1234
```

## 3. 注入与注释

### 一、注入
(1)PHP注入，'-'后面直接写PHP语句。请尽量不要使用，一定要使用请做好被TJJ的觉悟  
Jedi:
```shell
-$clever = BigSheep::isShaBiOrNot('yangminda')
```

对应的PHP:
```shell
<?php
$clever = BigSheep::isShaBiOrNot('yangminda');
?>
```

(2)JS注入，使用‘!‘实现，格式如下  
Jedi：
```shell
script
	!
		_defer.push('/follow.js');
		alert('bigsheep');
```

对应的HTML：
```shell
<script>
	_defer.push('/follow.js');
	alert('bigsheep');
</script>
```

### 二、注释

(1)注释一行  
Jedi:
```shell
//我是大绵羊
//双斜杠必须写在行首
```

对应的PHP:
```shell
<?php
//我是大绵羊
//双斜杠必须写在行首
?>
```

(2)注释一段，使用'--'可以注释掉整个段落。  
Jedi:
```shell
--div.bigsheep
	'大绵羊
	span
		'big sheep
```

对应的PHP:
```shell
<?php
/*
 *div.bigsheep
 *	'大绵羊
 *	span
 *		'big sheep
 */
?>
```

### 4. 外部方法与静态方法

(1)引用php函数  
Jedi:
```shell
:external is_array		（声明了外部函数is_array）
:if is_array(a)
	'Bigsheep
```

对应的PHP：
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

对应的PHP：
```shell
<?php
if (Category::exist($data->categoryName)) {
	echo 'Bigsheep';
}
?>
```

注意： external必须写在文件头部第一行。

使用external声明，进而引用外部方法、静态方法，可以有效避免注入，保护JJ。

# 第二章 home、listing与viewAd

本章着重介绍三大页面中涉及的Jedi使用方法。

保证新人阅读、修改三大页面的能力。

本教程系入门教程，不会涉及过多细节。

## 0. 变量、方法与表达式

### 一、成员变量与成员方法
（1）Jedi中成员变量用“.”连接  
Jedi：
```shell
"{ad.categoryEnglishName}
```

对应的PHP：
```shell
<?= $data->ad->categoryEnglishName); ?>
```

（2）成员方法  
Jedi：
```shell
"{ad.getPrice(adId)}
```

对应的PHP:
```shell
<?=  $data->ad->getPrice($data->adId); ?>
```

（3）‘$’会作为额外符号加入PHP变量名中  
Jedi：
```shell
“{$variableName}
```

对应的PHP：
```shell
<?= $data->$variableName?>
```

### 二、表达式

（1）Jedi中表达式的 运算符 与 优先级 同PHP  
Jedi：
```shell
"{(a + b) * !c}
```

对应的PHP:
```shell
<?=  ($data->a + $data->b) * !$data->c ?>
```

（2）使用’  if   then else ’ 表示 ‘?   :  ‘   
Jedi:
```shell
"{if  a > b then 'good' else 'bad'}
```

对应的PHP：
```shell
<?= $data->a > $data->b ? 'good' : 'bad' ?>
```

## 1. 模板继承

layout.jedi:
```shell
//layout.jedi是一个模板
head
	'我是head
#body
	// '#'代表在body标签上建立了钩子
	'我是body
```

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

实际应用：

三大页面同时import了layout.jedi。

Layout.jedi包涵基本的header、footer等。

## 2.闭包

（1）Jedi中的闭包类似于一个PHP函数，使用方法如下：  
Jedi：
```shell
:: time.friendly ()           	( 定义一个名称为time.friendly的闭包 )
		"{*}	        ( *代表从外部传入的参数 )
time.friendly =  '12345' 	( 引用闭包，参数为‘12345’ )
```


输出到页面上的结果：
```shell
12345
```

（2）闭包的参数可以是一个简单类型的变量，也可以是一个数组或一个实例。  
Jedi：
```shell
:: time.friendly ()           	
		"{*[0]}
		"{*[1]}	
time.friendly =  ['12345', '54321']
```

输出到页面上的结果：
```shell
12345
54321
```

# 第三章 坑与展望

明天下午hax召集的会议会讨论清楚这部分内容。

这里先不赘述。