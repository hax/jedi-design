关于 Jedi 的与 Jade 相同或相似的语言特性设计的说明：
* [为什么使用基于缩进的语法？](offside_rule)
* [为什么（只）为 id 属性和 class 属性设计了特殊语法（element#id 和 element.class）？](idcls_sugar)

关于 Jedi 的与 Jade 不同的语言特性设计的说明：
* [为什么只支持 element.class#id 而不支持 element#id.class 的写法？](idcls_order)
* [为什么 .class 和 #id 不像 Jade 那样默认生成 div 元素？](idcls_divitis)
* [为什么不用 Jade 的属性语法 element(attr1=value, attr2, ...) ，而要采用 @attr 的写法？](attr_syntax)
* [为什么 subtemplate 要使用 element.class = model 的语法，而不用类似 Jade 的 简单 mixin f(params) 形式？](mixin_cons)
* [为什么 fragment（即 Jade 中的 block）要使用 #frag 的语法，这是否会和 element#id 混淆？](fragment)
* [为什么 script 和 style 元素中的代码必须使用 ! 嵌套结构？](script_syntax)
* [为什么不支持直接在 script 中的插值？](script_interpolation)
* [为什么没有提供不 escape 的插值功能（即 Jade 中的 !{value}）？](unsafe)
* [为什么没有 include 指令？](include)
* [为什么需要 external 指令？](external)
* [为什么文本是以引号开头？](quot)
* [为什么不支持 ! 运算符？](symbol_or_word)