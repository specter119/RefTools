# Zotero 跨平台同步附件的实现

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=2 orderedList=false} -->
<!-- code_chunk_output -->

* [基础概念](#基础概念)
* [1. Zotero 官网同步服务](#1-zotero-官网同步服务)
* [2. Webdav 同步](#2-webdav-同步)
* [3. ZotFile 配合同步盘](#3-zotfile-配合同步盘)
* [4. 软链接配合同步盘](#4-软链接配合同步盘)
* [结语](#结语)

<!-- /code_chunk_output -->

Zotero 的题录同步通过注册账号实现(见[Zotero 开箱指南](startup.md))。本文列举了实现跨平台/设备的四种方法，以实现：跨 Windows/Mac/Linux 平台(设备)上的附件的同步，并对不同方法进行了简单的评价。

实现本文同步附件方法前，请参考[Zotero 开箱指南](startup.md)以保证 Zotero 正确安装配置。

## 基础概念

从附件类型上来看，Zotero 实现同步的方式不外乎两类：

- 同步`文件`型附件，包括：
  - Zotero 官网同步服务
  - Webdav 同步
  - 软链接配合同步盘
- 同步`文件链接`型附件，包括：
  - ZotFile 配合同步盘

> Zotero 附件类型包括：
> - `文件`: 图标为系统默认图标或者 adobe 红，是 Zotero 默认的附件格式，存放在`<数据存储位置>/storage` 内一个8位数字和字母的子目录中。
> - `文件链接`: 图标为白色加小铁链，通常由 ZotFile 生成，实际保存在`链接附件的根目录`下。
> - `url 链接`: 图标为蓝色加小铁链，实际为文件的网址，联网时才能打开。
>
> 因此，本地只保存有`文件`与`文件链接`类型的附件。

两类方法使用上的优劣：

- `文件`附件：
  - 劣势：路径自定义程度低，`<数据存储位置>/storage`或`软链接`内8位数字和字母组成的子目录。
  - 优势：删除题录，附件随之删除
- `文件链接`附件(特指由 ZotFile 生成):
  - 优势：路径自定义程度高
  - 劣势：删除题录，附件不会随之删除，`链接附件的根目录`概念略费解。

## 1. Zotero 官网同步服务

讲真，花钱省去配置的时间，也不是什么不好的事情。收费标准如下，供诸君考虑。

| Storage Amount | Annual Price (USD) |
|:--|:--|
| 300 MB | Free |
| 2 GB | $20 |
| 6 GB | $60 |
| Unlimited | $120 |

浏览器登录账号后，进入[Upgrade Storage](https://www.Zotero.org/settings/storage).

购买服务后，勾选`首选项`→`同步`→`设置`中文件同步下面两个选项。

> 配置难度: 极简，无需其他客户端
> 跨平台设备： 支持。
> 缺点: 继承`文件`附件缺点 + 如果你觉得贵。

## 2. Webdav 同步

Zotero 官方报道已经成功配置 Webdav 的服务见 [List of WebDAV services](https://www.zotero.org/support/kb/webdav_services)。此外，[坚果云](https://www.jianguoyun.com/)也支持 Webdav 的服务，配置方法见坚果云官方帮助：[webdav 连接坚果云](http://help.jianguoyun.com/?p=3168)，其收费方案见[坚果云收费方案](https://www.jianguoyun.com/s/pricing)(包括免费方案)。

> Papership 使用坚果云 WebDav
> 先将 Zotero 中的 webdav 重置，然后重置 Papership 的 webdav。
> 如果不行，在坚果云的 zotero 文件夹中创建空白的 lastsync.txt 文件，重试 Papership 的 webdav 连接。
> 参考 <http://bbs.jianguoyun.com/topic.php?id=6558#post-30230>

> 跨平台设备同步：支持，
> 配置难度: 简单，无需其他客户端
> 缺点: 继承`文件`附件缺点 + 不支持断点续传 + 单文件不可超过100MB(坚果云政策)。

## 3. ZotFile 配合同步盘

1. 配置附件链接根目录

    `链接附件的根目录`是`文件链接`附件的实际位置，当 Zotero 访问`文件链接`附件时，会访问此目录下的相对路径。为了达到同步附件的目的，需要将`链接附件的根目录`设置为同步盘的子目录。

    > 什么叫相对/绝对路径？
    > 并不难理解，但可能不常接触。
    > 对时间而言，明天是个相对日期，公元2011年11月11日是个绝对的日期（在地球上，没啥歧义）。
    > 路径也是如此，对 Windows 自 盘符:\ 开始，到文件夹或文件结束的就是文件的绝对路径，对 Linux 而言 / 开始（~ 开始也能算）就叫做相对路径，同样到文件夹或文件就叫做相对路径。
    > 看不懂也没关系，跟本文设置即可。

    修改配置，设置`首选项`→`高级`→`文件和文件夹`中`链接附件的根目录`。
    ![设置同步根目录](figs/sync_root_folder.png)

1. 下载安装 ZotFile

    for Zotero 5.x: [5.0.9](https://github.com/jlegewie/zotfile/releases/download/v5.0.9/zotfile-5.0.9-fx.xpi)，下载后在 Zotero 中打开`工具`→`插件`，按右上角齿轮选择`Install Add-on Form File ...`，选中刚刚下载的 `zotfile-x.x.x-fx.xpi` 文件进行安装(Mac 和 Win 版本可以拖拽，为了通用性，不再赘述)。
    ![安装插件](figs/install_plugin.png)

1. 配置 ZotFile

    打开`ZotFile Preferences ...`，`General Settings`标签页，`Source Folder for Attaching new Files`设置为`数据存储位置`下的`storage`。`Location of Files`设置为`链接附件的根目录`。

    ![配置 ZotFile](figs/zotfile_settings.png)

1. 同步\&Enjoy!

    开启同步就好了，对于已经存在本地的附件，请选中所有题录，右键 `Manage Attachments`→`Rename Attachments`。

### 其他

> 目前，webdav 打开后，会对 ZotFile 产生附件链接产生影响，原因暂时不明。

1. 软链接同步变更为 ZotFile 附件链接同步
  安装 ZotFile，**保持`附件链接根目录`不变**，剩下都按照本文叙述来，ZotFile 重命名后，再修改`附件链接根目录`。

1. 修改目录/更换网盘
  其实本质还是改目录，将`附件链接根目录`剪切到新位置，修改`附件链接根目录`和`Location of Files`为新位置即可。

> 跨平台设备同步：支持，
> 配置难度: 中等，
> 缺点: 继承`文件链接`缺点。

## 4. 软链接配合同步盘

> 软链接(symbolic link)翻译为符号链接更合适，但与之相对的概念是硬链接(hard link)，因此软链接这个叫法大行其道，这个叫法通俗，但并不形象。
> 它类似于 Windows 中的快捷方式，但更进一步。快捷方式只认你的鼠标双击，然而，软链接可以作为文件被其他应用访问，同样不怎么占地方。
>
> 参考创建软链接教程：
> - [windows 文件文件夹映射 junction 和 mklink，创建软硬链接](http://www.codes51.com/article/detail_223538.html)
> - [The Complete Guide to Creating Symbolic Links (aka Symlinks) on Windows](https://www.howtogeek.com/howto/16226/complete-guide-to-symbolic-links-symlinks-on-windows-or-linux/)

如果使用 linux/macOS，则只需打开终端模拟器。执行：

```sh
mv "<数据存储位置>/storage" "<同步盘中的 storage 位置>"
ln -s "<同步盘中的 storage 位置>" "<数据存储位置>/storage"
```

如果使用 Windows 系统，稍显麻烦，首先打开`命令提示符`:

> Windows 可能根据设置不同打开 PowerShell 不过对以下过程并不影响。

- for win7<sup>-</sup>: `win` + `R`, 输入`cmd`，回车
- for win8<sup>+</sup>: `win` + `X`, `I`

将`数据存储位置`storage 剪切到你能同步的位置，然后创建链接:
> Win7以上内置 mklink，但是对于 XP 及以下，需下载：[Junction 1.07](https://docs.microsoft.com/zh-cn/sysinternals/downloads/junction)

- for xp<sup>-</sup>: `<junction.exe 的完整路径> "<数据存储位置>/storage" "<同步盘中的 storage 位置>"`
- for win7<sup>+</sup>: `mklink /J "<数据存储位置>/storage" "<同步盘中的 storage 位置>"`

**强烈建议不要与 ZotFile 混用**，会让本方法变得更为复杂，且毫无意义。

> 跨平台设备同步：支持，
> 配置难度: 较难，
> 缺点: 继承`文件`附件缺点，配置难。

## 结语

以上四种方法皆可实现跨平台设备的附件同步，差别在于实现的难易及花费。它们均有可取之处，本人倾向使用 ZotFile 生成链接附件，配合同步盘同步的方法。同步盘的同步更便于移动设备的访问，Zotifle 的使用也加强了 Zotero 对附件的管理。

如有建议或问题，欢迎向 [RefTools/issues](https://github.com/specter119/RefTools/issues) 反馈，或在文末留言。
