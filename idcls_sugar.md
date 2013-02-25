这个设计其实是 follow CSS的设计——jedi里有许多语法是直接照搬 CSS 的。

这些地方包括：

E.classname
E#id
E > E
::before
::after

之所以给class和id特殊对待的原因其实和CSS为什么给出特殊语法的原因完全一样，因为这是最常用的，满足90%的场合。

考虑CSS中最常见的selector，当然是class和id选择器——尽管你也可以用attribute选择器。

从实际角度出发，模板源码中也会有大量的class和id。像现在的代码，几乎每个元素都有class或id（从我的角度来说其实是太多了），而大量元素除了class和id外都没有额外的attribute。仅仅从书写和阅读便利的角度，就应该提供特殊语法。

另外，从理论上说，class代表的意思是对element语义的细化（可以比照理解为OO中基类派生的子类型），id则是对element具体实例的标识（可以比照理解为一个具体的instance引用）；而除了极个别特例（如input上的@type）之外，普通的attribute只是元素的数据或元数据（可以比照理解为property）。在90%的情况下，class和id都是静态的（即由view的作者在template中写定），而attribute则可能是动态的（比如a@href的值或img@src的值来自于模型提供的数据）。

所以考虑到class/id与其他attribute这样泾渭分明的区别，给class和id以shortcut写法也就是合乎情理的，而且既然CSS已经这样设计了，考虑到模板的主要作者是熟悉CSS的前端程序员，沿用它是最好的选择。实际上我在这里的选择，只是沿用了jade沿用CSS的作法而已。至于说同时也可以用@class或@id，这只是从一致性上提供的。
