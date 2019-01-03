# 配置 VSCode 为简易 LaTeX 集成开发环境(IDE)

LaTeX的集成开发环境(Integrated Development Environment, 简称IDE)的配置方案非常多，专用软件(WinEdt, TexMakerX, TeXnicCenter)，或者是通用编辑器(vim, emacs, sublime text)均有。那么VSCode的优势到底在哪?

> [Visual Studio Code](https://code.visualstudio.com/)
> 简称VSCode，由微软开发的全平台集成开发软件。

1. 已经有专用编辑器了，为什么要用通用编辑器？
    因为不只写 LaTeX。
1. 相比传统『神级』编辑器(Vim, Emacs 等)，新一代编辑器(VSCode, Atom)有何优势？
    - 效率：传统编辑器高
    - 难度：新编辑器上手简单
    - 颜值：新编辑器高
1. Atom 和 VSCode孰优孰劣？
    - 支持：Atom插件多且好，包括且不局限于 LaTeX 范围。
    - 性能：VSCode好，启动快，大文件读写不卡(貌似和 LaTeX 没关系？)。
    - 难度：VSCode 上手难度比 Atom 略高。
    > Atom，装插件即用，默认设置很少修改，几乎都不知道中间发生了什么。但 VSCode，所有的配置都在`settings.json`中，使用者会更加明白发生了什么，包括默认设置做了什么。孰优孰劣，看各人喜好。

综上所述，VSCode的适合人群：同时具有编程和LaTeX需求，且对编辑器性能要求较高，有一定动手能力的人们。

## VSCode 安装之前的工作

[安装TeX Live套装](https://liam0205.me/texlive/)。

多看书`texdoc`, 多查 <https://tex.stackexchange.com/>，善于[提问](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)。

> CTeX/LaTeX 交流群，QQ群号: 31752345，中文开发者云集的群。
> 温馨提示，群里被群主虐过的才算萌新，否则只是渣渣。
> 进群先看群公告 × 3。

## VSCode 的安装配置

- Windows: 见[下载页面](https://code.visualstudio.com/download)，如使用绿色版且想从终端使用，将所在路径加入环境变量`%Path%`
- macOS: `brew cask install visual-studio-code` [brew](https://brew.sh/), [cask](https://caskroom.github.io/)
- Linux: [offical setup link](https://code.visualstudio.com/docs/setup/linux)

## 安装使用 [LaTeX Workshop](https://github.com/James-Yu/LaTeX-Workshop)

点选左侧扩展按钮，在展开的扩展栏顶端搜索 LaTeX Workshop，搜到后点击安装。
> 该扩展是当前编译 LaTeX 的全部依赖，微软官方的LaTeX Language Support已经停止开发。

使用方法可以通过 `Ctrl/Cmd` + `Shift` + `p` 命令快捷方式中，搜索 latex workshop 获得。

> 如下设置可实现 xelatex -> bibtex -> xelatex * 2 编译。

使用 `Ctrl/Cmd` + `,` 打开设置窗口，左侧为默认配置，右侧为用户的全局配置，在右侧加入(注意缩进)：

```json
    "latex-workshop.latex.magic.args": [
        "-shell-escape",
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "%DOC%"
    ],
```

主(根)tex文件开头，用魔法注释(Magic Comment)设置tex及bib编译命令。

```tex
% !TEX program = xelatex
% !BIB program = bibtex
```

非主(根)tex文件首行，建议用魔法注释定义 主(根)tex文件：

```tex
% !TEX root = 主文件绝对/相对路径
```

本文旨在采取尽量简洁的方法配置可用的 VSCode 的 LaTeX 写作环境，删除了之前冗余的 recipes 及 tools 的解释与配置，现在的魔法注释完全可以代替之前的方案，并且更加方便快捷。如需了解更多，请参考插件官方主页。

## 结语

至此，一个简单可用的 LaTeX 环境就搭建好了：

- 灵活更换引擎
- 保存即编译
- 软件内实现预览及同步
- 支持格式化
- 支持同步
- 插件无需配置
