# Zotero 的爱恨情仇 2018

> 拔剑吧少年，我的电脑里只容得下一款文献管理软件。
> 如果文献管理软件有名字，那一定是Z开头。

这篇不是教程，也没有长篇回顾历史，只是从 Zotero 官方的 blog 里扒出来几行很有趣的脚注，有感而水。如果你对非 Zotero 的文献管理软件有强烈偏执的爱，请尽早撤离，因为你上错了车。

## 兄弟软件的黑历史

大多真心喜欢 Zotero 的人都想推广它给更多的人用，于是一篇 “为什么选择 Zotero ？” 的腹稿|草稿|博客就产生，但写完了，发现并不吸引人。不过 Zotero 的爱好者们，你们不用愁了，官方写完了。

[Why Zotero?](https://www.zotero.org/why)

博客中，当然是说一些 Zotero 的优点，按照通常的套路，正文里需要一个对比功能的表，然后 Zotero 的绿对号数目以碾压之势领先其他文献管理软件。然而 Zotero 并没这么做，它把黑别人放到了脚注中，诸君请将网页滚至最下方，看脚注1。

> 1. Other reference managers go through long periods of little to no development that disrupt productivity and even prevent access to research data. For example, EndNote didn’t support Word 2016 [until 7 months after its release](https://twitter.com/EndNoteNews/status/694563436408143872). Mendeley has [taken years to support the latest versions of macOS](https://www.mendeley.com/release-notes/v1_18), and [took two months](https://blog.mendeley.com/2018/07/18/how-to-recover-your-files-and-annotations-in-mendeley-desktop-july-2018/) to address reports of PDFs disappearing from users’ libraries.

笔者水译：其他文献管理软件都经历过长时间的疏于开发，这有碍创作，甚至阻止对研究数据的访问（内心OS：不仅拦着我写文章，还拦着我看文献）。例如，EndNote 在 Word 2016 发布7个月后才支持（拦着我写文章）。Mendeley 花了数年才支持了 macOS 的最新版本，花了2个月才定位用户数据库中PDF文件丢失的报告（对，拦着我看文献的就是你了）。

正文里一本正经，脚注1黑 Mendeley 的**数年**让人拍案叫绝，捋下时间：

- 2016年7月7日，MacOS Sierra (10.12) 公开测试版放出
- 2016年9月20日，MacOS Sierra (10.12) 发布
- 2018年4月中旬（估计而来），Mendeley v1.18 发布，兼容 MacOS Sierra

也就是大概一年半的时间，macOS 用户是没法用 Mendeley 的，期间 Mendeley 论坛里苹果用户的怨言是很高。更有趣的是，在 Mendeley v1.18 发布后不久：

- 2018年6月4日 macOS Mojave 发布（应该只有新机预装）
- 2018年9月24日上线 Mac AppStore（供老机升级）

不过，Mendeley 似乎没再发生不兼容的情况，开发者应该捏了一把冷汗。

> 截至 2018.11.08(本文初稿写完)，Mendeley 最新版本 v19.2 与 macOS Mojave兼容性良好，不再提供 macOS Sierra/ windows XP 之前版本的支持。

这确实是一段比较传奇的黑历史，剩下两个一个7个月，一个2个月，加上没有亲身体验，不再展开了。

## 优秀，但说不出口

看完了这段高级黑的脚注，可以返回来看看 Zotero 怎么夸自己了，此时才明白，该文的重点放在开发团队非常的认真负责，很少提及软件的优秀。但这反而是扬长避短，因为在冰冷的功能对比里，它处于劣势，也是推广 Zotero 的文章开起来大都索然无味的原因。

**Zotero 功能上真没啥优点吗？那我们为啥还要用它？**

裸装的 Zotero，不比其他管理软件有能说出来的优势。

- 流畅？去统计别的软件在旧电脑的卡顿？
- 稳定性？统计别的软件崩溃次数？
- 兼容性？嗯，已经黑过了。
- 引用样式多？包括 Mendeley 和 Citavi 在内也都支持 CSL (即使 CSL 和 Zotero 才是本家)。
- 自定义程度高？能开飞机上天吗？
- 扩展性好？那能装意大利炮吗？

本着薅毛不换羊的策略，继续拿 Mendeley 来对比。客观的说，Mendeley 比裸装的 Zotero 更为优秀。

- 官方免费附件同步空间， 2GB (Mendeley) >> 300MB (Zotero)
- 自定义附件路径，支持（Mendeley）Vs 不支持（Zotero）
- 固定 Citation Key，支持（Mendeley）Vs 不支持（Zotero）

这么对比，Zotero弱爆了好吗？但加上自定义程度+扩展性呢？

- 第三方同步附件，不支持（Mendeley）Vs 支持（Zotero: WebDav|Zotfile|symbolic link）
- 自定义附件路径，支持（Mendeley）Vs 强大（Zotfile）
- 固定 Citation Key，支持（Mendeley）Vs 强大（Better BibTex）

对于附件同步来说，Mendeley要么用官方，要么不能开同步，这意味着，用户离开 Mendeley 官方同步很难在多设备间正常使用。后面两点，是基于 Zotero 的两个非常优秀的插件来实现，用强大形容他们功能，毫不过誉，简单的说，相近的功能，在 Mendeley 中，配置占据1个标签页，而在插件中，配置占据4个标签页。

经过这样对比，显得 Mendeley 非常羸弱，但事实上，在文献管理软件中 Mendeley 已经是一位优秀的勇者，最起码在易用性上，甩老牌文献管理软件 EndNote 好几条街，可如果和 Zotero 相比呢？对面也是位勇者，只不过穿上了神装。

## 酒香也怕巷子深

笔者从2013年左右接触 Zotero，当时 Zotero 已经处于 4.X 成熟阶段，但当时Zotero的学习资料非常少，大多数人都是看着[阳志平老师 Zotero 系列教程](https://www.yangzhiping.com/tech/zotero)慢慢入门的。

5.X 对于Zotero来说是非常大的更新，它重写了数据库，使得同步的稳定性大大增加。笔者看来，Zotero 开发者有着浓厚的匠人精神，重写数据库肯定要比加个花里胡哨的用户可以看到的功能要难多了，但缺能让软件走的更远。Zotero的开发重心完全在核心功能上，用户层面的功能，完全留给了插件开发者。这正造成了，优秀说不出来的窘境，总不能拿着别人开发的插件来吹 Zotero 多好用吧？

与此同时，在开源软件中，Zotero也少有的拥有异常丰富的文档，因为这通常是商业软件才会有的。丰富的文档，也在另一个层面上，削弱着宣发的动机，就是【开发者不想说话，给你甩过来一页 Zotero 文档】这种感觉。

综上，加上开源软件没有利益相关，Zotero 在国内至今也未听闻有大力度宣传，使得一款如此优秀的文献管理软件，鲜有人问津。

感兴趣的朋友，可关注[其 twitter](https://twitter.com/zotero) 以获取最新动态。

使用中需要帮助的朋友在官方论坛发帖，或加入以下2个QQ群之一（群友皆为义务帮助，请智慧的提问，以节省双方时间）：

- [Docear、Zotero、Mendeley群 群号：107948041](https://jq.qq.com/?_wv=1027&k=5q6w7oq)
- [Docear,Zotero,Mendeley 2 群号：82885024](https://jq.qq.com/?_wv=1027&k=5mmaJI1)

但是，QQ群确实不适用于软件的系统学习，而且，重复问题的比例也会很高，所以高质量，且不与官方文档重复的中文教程，能够对 Zotero 起到更好的推广作用。于是，笔者更呼吁更多人加入 Zotero 官方wiki的翻译，给本专栏填砖加瓦，包括硬核教程，翻译关键 Zotero Blog，乃至优秀灌水。

最后，希望更多的人用上 Zotero，爱上 Zotero。