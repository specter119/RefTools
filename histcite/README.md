# HistCite

HistCite 是一款非常优秀的引文分析软件。它由[Eugene Garfield](https://en.wikipedia.org/wiki/Eugene_Garfield)开发，后来被汤森路透公司收入旗下。而后和 WOS(Web of Science), EndNote 一起归了[Clarivate Analytics](https://clarivate.com/)，现在也终于停止开发。

> 事实上，在 HistCite 归汤森路透的时候，也几乎不开发了。个人认为 HistCite 上在功能上已经近乎完美（瑕不掩瑜？）。继续开发的方向可能只有支持更多的数据库了，但是这与汤森路透的利益向左。

下载及官方公告见: [HistCite: No longer in active development or officially supported
](https://support.clarivate.com/ScientificandAcademicResearch/s/article/HistCite-No-longer-in-active-development-or-officially-supported)

HistCite 功能极其强大，使用的方法可以参考以下博文/课件。

[引文分析软件 histcite 简介 - 罗昭锋的博文 - 科学网—博客](http://blog.sciencenet.cn/blog-304685-383399.html)

[文献分析工具 Histcite 介绍及使用 - 李红 - 浙江大学图书馆](http://libweb.zju.edu.cn/attachments/2012-05/07-1336097592-126671.pdf)

扩展支持他站(非 WOS)引文分析。

[From Scopus2Histocite，用 histocite 软件作从 scopus 网站二次引文分析 - leochina -简书](https://www.jianshu.com/p/47f9547187b4)

但是，HistCite 使用中会遇到几个麻烦的问题，且没有跨平台支持。本项目的其余篇幅将介绍问题的解决与非 windows 平台上的安装使用。

[在 Windows 环境下的导入过程](win.md)

[非 Windows 环境下的安装](wine.md)

## 独立开发的 macOS 应用

[HistCite Mac](https://github.com/yangnim21029/histcite-mac) 是参照 HistCite 12.03.17 工作流程独立开发的 macOS 书目计量工具，与原版作者没有隶属关系，不包含原版 HistCite 的程序代码或可执行文件。

它支持 macOS 13 及以上，Apple Silicon 与 Intel Mac 使用同一份 Universal DMG，无需 Windows、Wine 或虚拟机。可导入 WoS 标签式纯文本及 HCI，查看引用指标并使用 Graph Maker 绘图。原创代码采用 MIT，随附组件保留各自许可。

[下载](https://github.com/yangnim21029/histcite-mac/releases/latest) · [安装说明](https://github.com/yangnim21029/histcite-mac/blob/main/docs/mac-install.zh-CN.md) · [兼容性比较范围](https://github.com/yangnim21029/histcite-mac/blob/main/docs/compatibility.md)

当前社区版本未经过 Apple 公证，首次打开方式见安装说明。原版 Windows 程序的 Wine 安装方式继续见上面的非 Windows 安装教程。
