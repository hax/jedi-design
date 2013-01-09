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
div @name=“BigSheep” @nick=“大绵羊”
```shell

对应的HTML:
```shell
<div name="BisSheep" nick="大绵羊"></div>
```shell
