# 配置VSCode为LaTeX集成开发环境(IDE) - 初级版

LaTeX的集成开发环境(Integrated Development Environment, 简称IDE)的配置方案非常多，专用软件(WinEdt, TexMakerX, TeXnicCenter)，或者是通用编辑器(vim, emacs, sublime text)均有。那么VSCode的优势到底在哪?

> [Visual Studio Code](https://code.visualstudio.com/)
> 简称VSCode，由微软开发的全平台集成开发软件。

1. 已经有专用编辑器了，为什么要用通用编辑器？
    因为不只写LaTeX。
1. 相比传统『神级』编辑器(vim, emacs等)，新一代编辑器(VSCode, Atom)有何优势？
    - 效率：传统编辑器高
    - 难度：新编辑器上手简单
    - 颜值：新编辑器高
1. Atom和VSCode孰优孰劣？
    - 支持：Atom插件多且好，包括且不局限于LaTeX范围。
    - 性能：VSCode好，启动快，大文件读写不卡(貌似和LaTeX没关系？)。
    - 难度：VSCode上手难度比Atom略高。
    > Atom，装插件即用，默认设置很少修改，几乎都不知道中间发生了什么。但VSCode，所有的配置都在`settings.json`中，使用者会更加明白发生了什么，包括默认设置做了什么。孰优孰劣，看各人喜好。

综上所述，VSCode的适合人群：同时具有编程和LaTeX需求，且对编辑器性能要求较高，有一定动手能力的人们。

## VSCode安装之前的工作

[安装TeX Live套装](https://liam0205.me/texlive/)。

多看书`texdoc`, 多查<https://tex.stackexchange.com/>，善于[提问](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)。

> CTeX/LaTeX 交流群，QQ群号: 31752345，中文开发者云集的群。
> 温馨提示，群里被群主虐过的才算萌新，否则只是渣渣。
> 进群先看群公告 × 3。

## VSCode的安装配置

- Windows: 见[下载页面](https://code.visualstudio.com/download)，如使用绿色版且想从终端使用，将所在路径加入环境变量`%Path%`
- Mac: `brew cask install visual-studio-code` [brew](https://brew.sh/), [cask](https://caskroom.github.io/)
- Linux: [offical setup link](https://code.visualstudio.com/docs/setup/linux)

## 安装 [LaTeX Workshop](https://github.com/James-Yu/LaTeX-Workshop)

点选左侧扩展按钮，在展开的扩展栏顶端搜索LaTeX Workshop，搜到后点击安装。
> 该扩展是当前编译 LaTeX 的全部依赖，微软官方的LaTeX Language Support已经停止开发。

使用方法可以通过 `Ctrl/Cmd` + `Shift` + `p` 命令快捷方式中，搜索 latex workshop 获得。

现在的`LaTeX Workshop`配置，有些拗口，需要修改如下设置:

> `LaTeX Workshop`升级到5.x版本后，右下角弹窗可将`toolchain`自动转换为`recipes`的，参考 [from toolchain to recipe](https://github.com/James-Yu/LaTeX-Workshop#from-toolchain-to-recipe) 了解更多。

```yaml
latex-workshop.latex.recipes: 编译的方案
latex-workshop.latex.tools: 编译的命令及参数
latex-workshop.latex.magic.args: 魔法注释命令的参数
```

> 编译方案 ([Latex recipes](https://github.com/James-Yu/LaTeX-Workshop#latex-recipe)) 就是不同tools的组合，把 `latex-workshop.latex.tools`串起来。
> 魔法注释 ([Magic comments](https://github.com/James-Yu/LaTeX-Workshop#magic-comments))，用来定义编译引擎(pdflatex, xelatex 等)以及根文件 ([Root file](https://github.com/James-Yu/LaTeX-Workshop#root-file))。

下面举例实现 `xelatex` > `bibtex` > `xelatex` > `xelatex` 的配置。

使用 `Ctrl/Cmd` + `,` 打开设置窗口，左侧为默认配置，右侧为用户的全局配置，在右侧加入(注意缩进)：

```json
    "latex-workshop.latex.recipes": [
        {
            "name": "xelatex -> bibtex -> xelatex*2",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        }
    ],
    "latex-workshop.latex.tools": [
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
              "%DOCFILE%"
            ]
        },
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-shell-escape",
                "%DOC%"
            ],
        }
    ],
```

> `Build Latex Project`使用`latex-workshop.latex.recipes`中的第一个。
> `Build with recipe`可以手动选择`latex-workshop.latex.recipes`中的一个。
> 如果不设置`latex-workshop.latex.recipes`，默认将使用`latexmk`编译，可以参考 [Configuration Files](http://mg.readthedocs.io/latexmk.html#configuration-files) 了解更多。

非根tex文件，建议第一行前插入：

```tex
% !TEX root = 主文件相对路径
```

> 当前版本，**不建议使用魔法注释定义引擎**。
> 以前魔法注释定义的引擎相当于现在的`tools`，而现在定义的是只包含一步的`recipes`且优先级更高。所以，现在的魔法引擎使用情景非常有限，等待作者转换下思路。

> 个人觉得，现在的魔法注释，要么写`latexmk`这种集成编译命令，或者编译不含文献的文档时候可以随便换`pdflatex`或者`xelatex`。但是针对含文献的情况，反而不如之前的版本灵活。
> 如果诸位觉得现在的定义更好，可以留下你的方案给予建议。

## 结语

至此，一个简单可用的LaTeX环境就搭建好了：

- ~~灵活更换引擎~~(现在的魔法注释并不好用)
- 保存即编译
- 软件内实现预览及同步
- 支持格式化
- 支持同步

缺点：

- ~~不支持自定软件预览pdf~~(现在的说法是不官方支持)
- 不支持语法检查
