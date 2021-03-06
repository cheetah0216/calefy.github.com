---
layout: post
title: 关于css的思考
summary: css很简单，简单到可能不用一周的学习就能够开始使用；css又很复杂，做了几年还是发现很多问题没有遇到，很多新的思路可以去尝试⋯⋯在追求完美的路上，困顿、纠结与惊喜交织，明明感觉到豁然开朗就在前方，薄薄的那层迷雾始终无法看透。
category: css
tags: [css, 类名定义, 结构定义]
---

{{ page.title }}
================

{{ page.summary }}

原则
----

1. 清晰明了
	
	代码是给人看的，要适应日后阅读和改动维护，这点和程序一样，没有人愿意对着一团乱麻般的代码去寻找你创建时的思路。

2. 精简高效

	在 ```1``` 的基础上，减少不必要的冗余，提高效率就成了我们下一步要保证到的东西。精简就是要合并可以减少代码总量的相同属性，以及去掉已经通过继承得到而不需要再重新定义的属性；效率主要是选择符的使用上减少层级。


简单的一个页面或是几个页面组成的小网站，即使对一个初学者，css都不会构成什么问题，但是如果网站规模上升、开发团队变大、修改变动纷至沓来，如何保证css清晰明了、方便协作开发就成了不得不考虑的问题。

命名方式
--------

驼峰式、下划线分割、连字符分割，以及前面几种形式混搭是css名字可以达到的组合方式。

{% highlight css %}
.newsContentImage{...} /*驼峰式*/
.news_content_image{...} /*下划线分割*/
.news-content-image{...} /*连字符分割*/
.newsContent-image{...} /*混搭方式之一*/
{% endhighlight %}

用哪种方式，并没有优劣之分，清晰与否也完全取决于个人习惯。如果非要区分一下，驼峰式的优势是可以减少一个连字符所占据的字节，省下优化一张图片就足以节约出来的文件大小；下划线方式比中划线好的地方则是多数的编辑器都把这种方式分割的单词看做一个单词，双击就可以选中；连字符的分割则更符合国际化。

国外的网站多使用完整的 *单词+连字符* 组合，而国内，限于单词的掌握量，很多时候翻来覆去的主要区块不断的使用同样的单词命名，这点无可厚非，能表明位置、区分结构足矣，只要不是首字拼音的缩写，或是没有遵从缩写的习惯，都是可以的。关键是自己能够坚持自己一致的习惯，或者是团队约定好的习惯方式，保持整站的一致性。

我更倾向于使用全名或使用一些诸如 cnt 类的约定俗成的缩略方式，主要还是为了日后查看和修改时能够快速明白表达的意思。但是这里面又涉及到如果一个位置要表明意思要用到很多单词，全写上将是一个很长的名字，如果里面结构再复杂，对于追求简明的我们，这是一个很头疼的问题。与开发人员探讨，在程序中也存在同样的问题，但是他们很幸运，程序编译后就不存在这个问题了，即使是javascript，压缩后同样变成了很简短的名字。google网站上很多命名很简短，但我不知道实现方式，看上去像是经过了统一编译的，但有些地方还是很传统的方式，也可能是约定了一些东西。

如何简单明了的命名，并能在整个网站范围内保证灵活不冲突，是贯穿于整个工作过程中的。

css类名没有命名空间的概念，所有的类名（最外层）都是全局属性，对于一个只有几个页面的小网站来说没有什么问题，一旦遇到大的网站，而且还要不断改动（这也是互联网的特点）,再加上多个人协作开发，这将是灾难性的，混乱与冲突几乎不可避免。但是通常的方式，是我们在一个模块中始终带上最外层的类名，模拟命名空间的特点。


结构组织
--------

css应该如何划分？有些是约定俗成的，有些是自己的定义，但大体不外以下几种：

- 标记reset
- 全站通用的类，诸如 .l, .r, .p10 等等自定义的习惯方式
- 使用次数很多的公共模块，诸如 布局、自定义弹出框、表单、头像名片 等
- 小图标等sprite
- 在两个或两个以上模块页面公用的定义
- 各个页面的特别样式

我们可以按上面划分的方式，把每条分成一个文件，或是合并几条为一个文件。有的网站还要划分font、form等文件，这个就要根据自己网站的改动情况而定了，如何能更好的适应当前网站为准，如果足够小，我甚至建议放在一个文件里面就好了。

我现在一般分成四种文件：

>1. reset、通用类、layout。这三类一般比较稳定
>2. 全站使用次数很多的公共模块、sprite。甚至sprite可以再拆分出去方便日后变动
>3. 两个或两个以上模块页面用到的公共定义。
>4. 模块页面定义，如私信包含收件发件等一系列页面。这里面的都是该模块所独有的内容

2、3中的公共模块，应该拆分成按需加载才更完美。

ok，看上去很完美了。又会遇到什么样的问题呢？

在开始一个网站的构建之前，我们希望能够看到尽可能多的设计图，以便提取出公共的部分，甚至改正我们类名的定义。起码在我现在的公司是不现实的，很多时候是同步开发，最多只能看到一个模块的系列页面，造成的后果就是要不断根据后续设计修改前面的代码结构。

这还不是最严重的，无非就是加大工作量，重新规整。网站日趋庞大，过了集中设计时期，产品与UI设计出的东西可能是前面某些公共模块的部分组合，如果人员变动，都已经不记得之前有过什么，面对日益冗余的代码，我们也渐渐不敢去改动，宁愿去累加保证安全也不再愿意去重新规整代码。出现这种问题的原因很明显，没有统一的UI设计，尽管我们不断反馈，但是相信很多公司都会面临这种问题，这也是诸如css框架不能使用的原因。

我们渴望像 [Bootstrap](http://twitter.github.com/bootstrap/) 那样整出网站的通用模板，但这需要从产品到设计整个流水线有统一的认识，一直在呼吁，但是短期内是不现实的。


结论
----

基于现状，我们只能给出最好的解决方案，但是对于后续维护，在速度和质量上追求平衡，更多的取决于制作者本人的意愿。

一个好的方式是像豆瓣那样，外层有全局的类名，内部定义一些特定的简短类名表明结构，比如 `.hd`、`.bd`、`.ft` 之类的结构。对于内部的详细样式可以用些简短单词定义，但也要约定一些关键词性质的词语不暴露在全局定义中。这个定义的详细程度根据具体的项目而定。

规范的作用是让团队每个人都有共同的格式和结构定义方式，但有时会发现严格遵守规范可能会限制思路，这时候要具体记录，拿出来商讨，反馈回规范，看看问题出在何处。规范也是一个不断迭代改进的东西，但最基本的原则是固定的，那就是保证我们的代码高效、清晰、精简。

每个人的工作内容不同，实际情况也不同，这里只是个人的想法，欢迎探讨。


**附* 如果用.hd/.bd/.ft表明一个模块结构时，里面有嵌套了模块的时候，简短类名有冲突又要如何修正？thinking...

