# HistCite
## 1. 安装
```sh
brew cask install xquartz
brew install wine --devel
brew install winetricks
```
将HistCite的可执行文件放在`~/.wine/dirve_C/Program Files/HistCite`，路径可自选，没有空格更方便。

制作`C:\fakpath`软链接

```sh
ln -s ~/Documents/Dropbox/应用/HistCite ~/.wine/drive_c/fakepath
ln -s ~/Documents/Dropbox/应用/HistCite ~/Documents/HistCite
```
>HistCite的hci文件必须从`C:\fakepath`导入，因此将hci所在文件夹向wine的C盘做一个软链接。在`~/Documents/HistCite`做软链接的目的只是为了好找。

运行可执行文件，并导入`.hci`文件进行测试。

## 2. 封装为app
打开Automator，新建app，自脚本

```sh
export PATH="/usr/local/bin":$PATH
wine ~/.wine/drive_c/Program\ Files/HistCite/HistCite.exe
```
保存为Histcite.app就可以调用了。

##3. 合并文件脚本
!注意，该脚本会合并`~/Downloads`下所有的txt文件，使用前务必保证该文件夹内txt文件都是刚刚下载的。

```sh
# !/bin/env bash
# conding:utf-8
echo "FN Thomson Reuters Web of Knowledge">~/.wine/drive_c/fakepath/histmerge.hci
echo 引文合并中...
for sprtfile in $(ls ~/Downloads/*.txt)
do
  tail -n +2 $sprtfile >> ~/.wine/drive_c/fakepath/histmerge.txt
done
echo ".#" >> ~/.wine/drive_c/fakepath/histmerge.txt
echo 引文已合并至"~/.wine/drive_c/fakepath/histmerge.hci"
```

## to-do list
* app可以打开关闭。
* 支持直接打开hci文件(貌似只在win上实现过，测试直接wine打开无反应)。
* 检索，合并结果的自动化，更多网站的兼容。
