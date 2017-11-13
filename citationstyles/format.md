# 初认Zotero(Mendeley, Docear, ...) 参考文献格式化

> 基于CSL语言的参考文献管理软件，对参考文献书目格式的控制

对大多数人而言，"插文献"是很多人使用文献管理软件的主要，乃至唯一的需求。除去跟风装软件的懵懂少年，人们使用文献管理软件的目的是什么？

用**简洁，单一**的方式，应付学位论文，期刊杂志，出版书籍中，**种类繁复，格式复杂的参考文献及数目的要求**。

本文以Zotero为例，介绍了现在"流行的"文献管理软件，控制Word中参考文献及数目格式的方法，作用范围及顺序。帮助大家在使用此类软件时，可以更轻松更精确的控制参考文献及数目的格式。

> "流行的"指的是以Citation Style Language (CSL)为基础的一类参考文献软件，相对经典的EndNote而言，Zotero, Mendeley, Docear等软件均属于此类。
> Citation Style Language是一种基于XML的广泛使用的开源语言，用于定义引用和书目的格式。
> CSL 主页 <http://citationstyles.org/>
> Zotero citation styles 介绍 <https://www.zotero.org/support/zh/dev/citation_styles>

## 1. 分清citation和bibliography

> 和两个单词差这么多，你居然要我分清？！

看英文单词，他们长得完全不一样，但在中文语境中沟通起来，一般只会用『参考文献』带过。所以，为了区分二者，在后文中做出如下规定：

Citation称为**参考文献索引**，或简称**索引**。它就是正文中的[1~3,5]或者somebody (year)这样的东西。

> 像路标一样，它告诉你，哪里引了哪篇文章，被引文章的具体信息，需要你在文末的bibliography中寻找。
> 它存在的意义是告诉读者，正文的那些内容引用了哪些文献，即交代内容与引文的对应关系。

Bibliography成为**参考文献书目**，或简称**书目**。它就是你正文往后，"References"或者"参考文献"部分的内容。这部分一般包括了被引文献的作者，年，期刊或出版社，[DOI](https://www.doi.org/)，[ISBN](https://zh.wikipedia.org/zh-cn/%E5%9B%BD%E9%99%85%E6%A0%87%E5%87%86%E4%B9%A6%E5%8F%B7)以及卷，期，页码等信息。

> 这些信息对被引文献做出了足够详尽的描述，能够帮助读者精确的锁定被引文献。

## 2. 参考文献索引的格式化

其实前面介绍定义时，举例中已经显示出，索引有两种格式：数字(number)与(author-year)作者年代。字面意思已经很通俗易懂了，CSL的一个分类方式也是按照索引的格式来的。

但是，对于作者年代格式，需要多交代一句。很多期刊有这样的要求(一般使用作者年格式的期刊均有)：

* 句子中提到某个人时(即人名充当句子主谓成分时)，索引写做author(year)；
* 引文对整个句子解释时(即人名不充当句子主谓成分时)，索引写做(author year)。

本文建议的处理是，在Word的Zotero选项卡中，点击"Add/Edit Bibliography"![](https://www.zotero.org/support/_media/word_integration/zotero-toolbar-word-add-edit-bibliography-5.png?w=16&cache=nocache&tok=1ba75c)

在弹出的书目编辑器中选择