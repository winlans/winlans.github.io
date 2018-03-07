---
title: es定义动态mapping
date: 2018-3-7 21:00:00
categories:
- elastic
tags:
- elastic
---

elastic非常易于使用， 甚至都不用我们自己创建mapping， 它也能自动的帮我们创建。但是有时候es的默认行为可能并不是我们想要的， 或者我们要指定使用哪个`analyzer`这时我们都需要手动的创建mapping。如果我们要索引的文档的数据差异比较大，可能我们需要定义很多类型的mapping， 但是为了偷懒，我们可以只指定我们需要约束的几个字段， 其他字段的属性让es自动为我们创建， 这就需要用到动态模板。 这就需要用到动态模板。

<!-- more -->

关于dynamic mapping其实没什么好说的， es的文档已经是很详尽了， 在这里我主要是想说下[es5.*](https://www.elastic.co/guide/en/elasticsearch/reference/5.6/default-mapping.html)与[es6.*](https://www.elastic.co/guide/en/elasticsearch/reference/5.6/default-mapping.html)的区别， 由于es近一年的更新速度非常快， 所以很多东西都已经改变了， 比如2.*版本还有`string`类型， 但5.*就被废弃了。

可以看到在最新的版本中， es已经废除了多type的支持， 之前我们一直认为es的存储结构很类似关系型数据库中的`database-table-row`的概念，由此而引发了不少的疑惑, 现在es终于将其废弃了。

现在可以在`mappings:_doc`中指定字段的行为， 匹配规则几乎是一致的。[点击查看](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/dynamic-templates.html)