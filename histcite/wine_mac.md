#HistCite
##1. 安装
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
##2. 添加命令  
在`~/.zshrc`最后添加:

```sh
alias histcite="wine ~/.wine/drive_c/Program\ Files/HistCite/HistCite.exe"
```

##3. to-do list
* HistCite txt 合并的命令
* 用Automator将histcite封装为app，可以打开关闭。
* 检索，合并结果的自动化，更多网站的兼容。