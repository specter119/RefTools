# Zotero跨平台同步附件的实现

Zotero的条目同步通过注册账号实现(见[Zotero开箱指南](startup.md))。本文列举了实现跨平台/设备的四种方法，以实现：

- 不同设备
- Windows
- Mac
- Linux

上的附件的同步，并对不同方法进行了简单的评价。

实现本文同步附件方法前，请参考[Zotero开箱指南](startup.md)以保证Zotero正确安装配置。

## 1. Zotero官网同步服务

讲真，花钱省去配置的时间，也不是什么不好的事情。收费标准如下，供诸君考虑。

| Storage Amount | Annual Price (USD) |
|----------------|--------------------|
| 300 MB | Free |
| 2 GB | $20 | Select Plan |
| 6 GB | $60 | Select Plan |
| Unlimited | $120 |

浏览器登录账号后，进入[storage](https://www.zotero.org/settings/storage).

购买服务后，勾选`首选项`→`同步`→`设置`中文件同步下面两个选项。

> 配置难度: 极简
> 跨平台设备： 支持
> 缺点: 一个附件对应一个目录，分享文件稍有麻烦

## 2. Webdav同步

设置方法见坚果云官方帮助：[webdav连接坚果云](http://help.jianguoyun.com/?p=3168)

[坚果云收费方案](https://www.jianguoyun.com/s/pricing)(含免费方案)，充值前请先参考：[坚果云不再续费后的空间和流量如何计算](http://www.jianguoyun.com/s/help/?p=1582)。

> 跨平台设备同步：支持
> 配置难度: 简单
> 缺点: webdav接口不支持断线续传，单文件不得超过10MB，坚果云的政策

## 3. Zotfile生成附件链接配合同步盘

先明确一些概念：

> `数据存储位置`和`链接附件的根目录`:
>>`数据存储位置`是Zotero本地几乎所有的配置、文献数据库、插件数据库以及附件存放的地方，基本上，除了Zotero及插件的主体程序都在此，同步的意义并不大。该目录下的`storage`子目录，存放有本地所有`文件`格式的附件，我们需要将`storage`设置为zotfile的监听目录。
> `链接附件的根目录`是Zotero保存文件链接的目录，当zotero以文件链接形式保存附件时，会记住在这目录下找相对路径。
>
> 附件类型
>> `文件`: 图标为系统默认图标或者adobe红。是Zotero默认的附件格式，存放在storage下一个8位数字和字母组成的文件夹内，同文件夹下`.zotero-ft-cache`是附件的内容或提纲，`.zotero-ft-info`是附件文件的一些信息。
> `文件链接`:图标为白色加小铁链。zotfile生成的链接即文件链接，实际保存在`链接附件的根目录`下。
> `url链接`:图标为蓝色加小铁链。保存网络文件链接，联网时才能可以打开。

1. 配置附件链接根目录
  修改配置，设置`首选项`→`高级`→`文件和文件夹`中`链接附件的根目录`，这就是附件最终存放的位置，因此是同步盘能同步的位置。
  ![设置同步根目录](figs/sync_root_folder.png)

1. 下载安装Zotfile
  for Zotero 4.x: [4.2.8](https://addons.mozilla.org/firefox/downloads/latest/zotfile/type:attachment/addon-284723-latest.xpi), for Zotero 5.x: [5.0.4](https://github.com/jlegewie/zotfile/releases/download/v5.0.4/zotfile-5.0.4-fx.xpi)
  下载后在zotero中打开`工具`→`插件`，按右上角齿轮选择`Install Add-on Form File ...`，选中刚刚下载的`zotfile-x.x.x-fx.xpi`文件进行安装(Mac和Win版本可以拖拽，为了通用性，不再赘述)。
  ![安装插件](figs/install_plugin.png)

1. 配置zotfie
  打开`ZotFile Preferences ...`，`General Settings`标签页，`Source Folder for Attaching new Files`设置为`数据存储位置`下的`storage`。`Location of Files`设置为`链接附件的根目录`。
  ![配置zotfile](figs/zotfile_settings.png)

1. 同步\&Enjoy!
  开启同步就好了，对于已经存在本地的附件，请选中所有条目，右键`Manage Attachments`→`Rename Attachments`。

### 其他

> 目前，webdav打开后，会对Zotfile产生附件链接产生影响，原因暂时不明。

1. 软链接同步变更为Zotfile附件链接同步
  装zotfile，**除了`附件链接根目录`不动**，剩下都按照本文叙述来，zotfile重命名后，再修改`附件链接根目录`。

1. 修改目录/更换网盘
  其实本质还是改目录，将`附件链接根目录`剪切到新位置，修改`附件链接根目录`和`Location of Files`为新位置即可。

> 跨平台设备同步：支持
> 配置难度: 中等
> 缺点: Zotero中删除条目，附件不会随之删除，需要手动删除。

## 4. 软链接配合同步盘

> 软链接(symbolic link)翻译为符号链接更合适，但与之相对的概念是硬链接(hard link)，因此软链接这个叫法大行其道，这个叫法通俗，但并不形象。
> 它类似于Windows中的快捷方式，但更进一步。快捷方式只认你的鼠标双击，然而，软链接可以作为文件被其他应用访问，同样不怎么占地方。
>
> 参考创建软链接教程：
>> [windows 文件文件夹映射junction和mklink，创建软硬链接](http://www.codes51.com/article/detail_223538.html)
>> [The Complete Guide to Creating Symbolic Links (aka Symlinks) on Windows](https://www.howtogeek.com/howto/16226/complete-guide-to-symbolic-links-symlinks-on-windows-or-linux/)

打开`命令提示符`:

- for win7-: `win` + R, 输入cmd，回车
- for win8+: `win` + X, I

将`数据存储位置`storage剪切到你能同步的位置，然后创建链接:
> Win7以上内置mklink，但是对于XP及以下，需下载：[Junction 1.07](https://docs.microsoft.com/zh-cn/sysinternals/downloads/junction)

- for xp- : `<junction.exe的完整路径> "<数据存储位置>/storage" "<云盘中的storage位置>"`
- for win7+: `mklink /J "<数据存储位置>/storage" "<云盘中的storage位置>"`

建议不要与zotfile混用，会让本方法变得更为复杂。

> 跨平台设备同步：支持
> 配置难度: 较难
> 缺点: 配置难，继承官方同步的缺点

## 结语

以上四种方法皆可实现跨平台设备的条目同步，差别在于实现的难易及花费。它们均有可取之处，本人倾向使用Zotfile生成链接附件，配合云盘同步的方法。云盘的同步更便于移动设备的访问，Zotifle的使用也加强了Zotfile对附件的管理。

如有建议或问题，欢迎向[RefTools/issues](https://github.com/specter119/RefTools/issues)反馈，或在文末留言。