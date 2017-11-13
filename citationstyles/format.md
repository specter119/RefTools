# 初识Zotero(Mendeley, Docear, ...) 参考文献格式化

> 基于CSL语言的参考文献管理软件，对参考文献书目格式的控制

对大多数人而言，"插文献"是很多人使用文献管理软件的主要，乃至唯一的需求。除去跟风装软件的懵懂少年，人们使用文献管理软件的目的是什么？

用**简洁，单一**的方式，应付学位论文，期刊杂志，出版书籍中，**种类繁复，格式复杂的参考文献及数目的要求**。

本文以Zotero为例，介绍了现在"流行的"文献管理软件，控制Word中参考文献及数目格式的方法，作用范围及顺序。帮助大家在使用此类软件时，可以更轻松更精确的控制参考文献及数目的格式。

> "流行的"指的是以Citation Style Language (CSL)为基础的一类参考文献软件，相对经典的EndNote而言，Zotero, Mendeley, Docear等软件均属于此类。
> Citation Style Language是一种基于XML的广泛使用的开源语言，用于定义引用和书目的格式。
> CSL 主页 <http://citationstyles.org/>
> Zotero citation styles 介绍 <https://www.zotero.org/support/zh/dev/citation_styles>

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=3 orderedList=false} -->
<!-- code_chunk_output -->

* [1. 分清citation和bibliography](#1-分清citation和bibliography)
* [2. 参考文献索引的格式化](#2-参考文献索引的格式化)
* [3. 参考文献书目的格式化](#3-参考文献书目的格式化)
	* [3.1 Zotero的题目(title)字段](#31-zotero的题目title字段)
	* [3.2 Csl文件的使用及修改](#32-csl文件的使用及修改)
	* [3.3 Word插件中使用书目编辑器](#33-word插件中使用书目编辑器)
	* [3.4 Word中的"书目"样式](#34-word中的书目样式)

<!-- /code_chunk_output -->

## 1. 分清citation和bibliography

> 两个单词差这么多，你居然要我分清？！

看英文单词，他们长得完全不一样，但在中文语境中沟通起来，一般只会用『参考文献』带过。所以，为了区分二者，在后文中做出如下规定：

Citation称为**参考文献索引**，或简称**索引**。它就是正文中的[1~3,5]或者somebody (year)这样的东西。

> 像路标一样，它告诉你，哪里引了哪篇文章，被引文章的具体信息，需要你在文末的bibliography中寻找。
> 它存在的意义是告诉读者，正文的那些内容引用了哪些文献，即交代内容与引文的对应关系。

Bibliography称为**参考文献书目**，或简称**书目**。它就是你正文往后，"References"或者"参考文献"部分的内容。这部分一般包括了被引文献的作者，年，期刊或出版社，[DOI](https://www.doi.org/)，[ISBN](https://zh.wikipedia.org/zh-cn/%E5%9B%BD%E9%99%85%E6%A0%87%E5%87%86%E4%B9%A6%E5%8F%B7)以及卷，期，页码等信息。

> 这些信息对被引文献做出了足够详尽的描述，能够帮助读者精确的锁定被引文献。

## 2. 参考文献索引的格式化

其实前面介绍定义时，举例中已经显示出，索引有两种格式：数字(number)与(author-year)作者年代。字面意思已经通俗易懂，CSL的一个分类方式也是按照索引的格式来的。

但是，对于作者年代格式，需要多交代一句。很多期刊有这样的要求(一般使用作者年格式的期刊均有)：

* 句子中提到某个人时(即人名充当句子主谓成分时)，索引写做author(year)；
* 引文对整个句子解释时(即人名不充当句子主谓成分时)，索引写做(author year)。

本文建议的处理是，在Word的Zotero选项卡中，点击"Add/Edit Citation"![](https://www.zotero.org/support/_media/word_integration/zotero-toolbar-word-add-edit-citation-5.png?w=16&cache=nocache&tok=a54d12)

在弹出的索引编辑器中勾选

![索引编辑器](https://www.zotero.org/support/_media/word_integration/edit_citation.png?cache=nocache)

* [x] 略去作者/Suppres Author

这样会生成一个(year)格式的索引，作者则直接写入正文。

## 3. 参考文献书目的格式化

书目的格式化，尤其是csl文件的修改，是很复杂的，本文不会详细展开，但日后会逐渐补充好的教程供大家参考，也不排除另开一篇去写一下，但都是后话。

书目的格式化，按照2个层次，推荐4种方法给大家，建议顺序使用。

* 内容+字体格式
  * Zotero的题目(title)字段
  * Csl文件的使用及修改
  * Word插件中使用书目编辑器
* 段落样式
  * Word"书目"样式

### 3.1 Zotero的题目(title)字段

可以插入html语言的标签来达到给title字体格式的效果，一般用于化学/数学式中的上下角标，效果如下：

* `稳重的题目<sup>上标</sup>` 效果: 稳重的题目<sup>上标</sup>

* `积极的题目<sub>下标</sub>` 效果: 积极的题目<sub>下标</sub>

下划线、粗体和斜体都也是html标签支持的，但一般来说，没有必要。

> 标题的修改仅限于零星特殊格式的字符，主要格式控制在csl文件

### 3.2 Csl文件的使用及修改

csl文件对于书目的在整个书目格式化的过程中，占据最主要的地位。简而言之，它定义了2个东西。

* 书目的组成与先后顺序
* 书目各组成的字体格式
  * 字体格式主要包括，上下标，粗斜体，以及不同规则字母大写

Citation style在线数据库有大量预定义的格式(本文写作时，zotero库中有8192种格式)，可以通过Zotero首选项直接安装，且现有的格式可以满足绝大多数需求。

对于已经安装好的csl格式，在Word插件中点击`Document Preferences`可以应用或更换格式。

但是当你没有找到合适的格式文件，可以选择修改现有的格式，具体的过程与方法，就不在此展开了，新手编辑csl文件使用 <http://editor.citationstyles.org> 的`Visual editor`编辑。

> [Onlie Visul editor 使用指南](https://github.com/citation-style-language/csl-editor/wiki/User-guide-for-the-CSL-Editor)

### 3.3 Word插件中使用书目编辑器

不论由于什么原因，通过前两种方法始终都无法获得你想要的字体格式，那么应该选择使用Word中的插件中的书目编辑器，而不是手动改。

> 喜欢手改的人，在评论区讲出你的故事。

### 3.4 Word中的"书目"样式

至此，我们已经彻底搞定了书目的内容和字体格式(注意，字体格式中并未包括字体大小)。那么，书目的字体大小，缩进，行间距由谁控制？

Word中参考文献书目应用了`书目`(Mac与英文环境或为`Bibliography`)样式，可以尝试shi用`Alt` + `Shift` + `Ctrl/Cmd` + S 呼出样式窗口，编辑其中的段落样式，保存后刷新书目，参考文献书目段落样式将更新为你修改后的"书目"，且不随你刷新失效。

> [微软：编辑样式](https://support.office.com/zh-cn/article/%E5%BA%94%E7%94%A8%E3%80%81%E6%9B%B4%E6%94%B9%E3%80%81%E5%88%9B%E5%BB%BA%E6%88%96%E5%88%A0%E9%99%A4%E6%A0%B7%E5%BC%8F-1a2cead9-897f-48a7-9122-7849d3b5030a)
> 缩进始终不正确的，尝试删除书目中所有的制表位。[微软：编辑制表位](https://support.office.com/zh-cn/article/%E8%AE%BE%E7%BD%AE%E3%80%81-%E6%B8%85%E9%99%A4%EF%BC%8C%E6%88%96%E5%88%A0%E9%99%A4%E5%88%B6%E8%A1%A8%E4%BD%8D-06969e0f-2c81-4fe0-8df5-88f18087a8e0)

通过本文的简单介绍，可以宏观了解Zotero等文献管理软件是如何控制参考文献索引与书目的格式，还能够针对不同的格式需求，提供寻找相应的解决方法的方向。希望通过本文的梳理，帮助到更多陷入使用类似软件，却不知道如何上手修改格式的朋友。欢迎更多的意见和建议。
