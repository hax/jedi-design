# 为什么不支持 ! 运算符？

【更新】最近的版本已经支持，但下述设计考虑仍适用。


没有第一时间支持的主要原因是想提供更好的替代语法，避免使用 ! 这样可能令 PM/Designer 不太习惯的语法，也尽量避免写出双重否定等容易出 bug 的条件判断：

【将 PM 和 Designer 作为 view 代码的读者甚至直接作者，这是我设计 Jedi 的许多细节时的一个重要考量——也是我为什么乐意在百姓网做这件事情，因为从我了解和沟通来看，我们的 PM 非常愿意去读甚至动手写 view 的代码，并且我们的文化鼓励这样。】

代码可读性我举个例子，在信息展示页面中有这样的代码（大意）：

```jedi
:if !isset(userAds)
    -- 显示相关信息（具体代码省略）
:else if count(userAds)
    -- 显示该用户发的其他信息（具体代码省略）
```

其中第一个 if !isset 其实很拗口，我个人的体验是读代码时会楞一下。

改成这样会稍微好那么一点：

```
:unless isset(userAds)
```

以及

```
:else if not empty(userAds)
```


最理想或许是改成这样：

```
:if userAds is undefined
  ...
:else if userAds is not empty
  ...
```

这里用了一些计划增加的内置特性，如 undefined 和 empty 检测。


从设计思路来说，我不想加上 ! 运算符，而是打算加上 not、and、or、is、is not 等运算符、 unless 结构（部分特性已经实现了）以及语言内置的 undefined、empty 检测。

注：之所以现在 release 的版本加入了 &&、|| 运算符而没有加 ! ，其实是从翻译现有的模板页面的实际情况出发的 ——
&& 和 || 用到比较多，且没有的话完全不能满足需求，而 ! 用的非常少，并且有很容易的 workaround ，就是写成 :if isset(userAds) == false 。
