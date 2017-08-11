# 配置VSCode为LaTex集成开发环境(IDE) - 初级版

LaTex的集成开发环境(Integrated Development Environment, 简称IDE)的配置方案非常多，专用软件(WinEdt, TexMakerX, TeXnicCenter)，或者是通用编辑器(vim, emacs, sublime text)均有。那么VSCode的优势到底在哪?

> [Visual Studio Code](https://code.visualstudio.com/)
> 简称VSCode，由微软开发的全平台集成开发软件。

1. 已经有专用编辑器了，为什么要用通用编辑器？
    因为不只写LaTex。
1. 相比传统『神级』编辑器(vim, emacs等)，新一代编辑器(VSCode, Atom)有何优势？
    * 效率：传统编辑器高
    * 难度：新编辑器上手简单
    * 颜值：新编辑器高
1. Atom和VSCode孰优孰劣？
    * 支持：Atom插件多且好，包括且不局限于LaTex范围。
    * 性能：VSCode好，启动快，大文件读写不卡(貌似和LaTex没关系？)。
    * 难度：VSCode上手难度比Atom略高。
    > Atom，装插件即用，默认设置很少修改，几乎都不知道中间发生了什么。但VSCode，所有的配置都在`settings.json`中，使用者会更加明白发生了什么，包括默认设置做了什么。孰优孰劣，看各人喜好。

综上所述，VSCode的适合人群：同时具有编程和Latex需求，且对编辑器性能要求较高，有一定动手能力的人们。

## VSCode安装之前的工作

[安装TeX Live套装](https://liam0205.me/texlive/)。
多看书`texdoc`, 多查[https://tex.stackexchange.com/](https://tex.stackexchange.com/)，善于[提问](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)。

> CTeX/LaTeX 交流群, QQ群号: 31752345，中文开发者云集的群。
> 温馨提示，群里被群主虐过的才算萌新，否则只是渣渣。

## VSCode的安装配置

* Windows: 见[下载页面](https://code.visualstudio.com/download)，如使用绿色版且想从终端使用，将所在路径加入环境变量`%Path%`
* Mac: `brew cask install visual-studio-code` [brew](https://brew.sh/), [cask](https://caskroom.github.io/)
* Linux: [offical setup link](https://code.visualstudio.com/docs/setup/linux)

## 安装 LaTex Workshop

点选左侧扩展按钮，在展开的扩展栏顶端搜索LaTex Workshop，搜到后点击安装。
> 该扩展是当前编译LaTex的全部依赖(微软官方的LaTeX Language Support已经停止开发)。

需要修改的主要内容，位于设置中`latex-workshop.latex.toolchain`，它是LaTex Workshop编译LaTex文件的命令串。本文通过设置 [Magic comments](https://github.com/James-Yu/LaTeX-Workshop#magic-comments) 和 [latex-toolchain](https://github.com/James-Yu/LaTeX-Workshop#latex-toolchain)，来实现 `xelatex` > `bibtex` > `xelatex` > `xelatex` 的编译过程。

使用 `[Ctrl/Cmd + ,]` 打开设置窗口，左侧为默认配置，右侧为用户的全局配置，在右侧加入(注意缩进)：

```json
    "latex-workshop.latex.toolchain": [
        {
            "command": "",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-shell-escape",
                "%DOC%"
            ]
        },
        {
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        },
        {
            "command": "",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-shell-escape",
                "%DOC%"
            ]
        },
        {
            "command": "",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-shell-escape",
                "%DOC%"
            ]
        }],
```

在tex的主文件，第一行前插入:

```tex
% !TEX program = xelatex
```

此时`latex-workshop.latex.toolchain`中留空(`""`)的`command`将被`xelatex`代替，形成 `xelatex` > `bibtex` > `xelatex` > `xelatex` 编译过程。

反之，当主tex文件中不设置`% !TEX program`时候，将使用`latexmk`的默认设置`pdflatex`代替。这中方案可以较为灵活的切换`xelatex`与`pdflatex`编译。

> 注意，如果你的`latexmk`的环境变量已经改掉了编译引擎，不设置`% !TEX program`也不再调用`pdflatex`，而是你修改后的引擎。

其他tex文件，推荐第一行前插入：

```tex
% !TEX root = 主文件相对路径
```

## 结语

至此，一个简单可用的Latex环境就搭建好了，下一篇的内容可能是代码格式化以及语法检查。
