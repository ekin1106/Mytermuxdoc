![termux](https://github.com/myfreess/Mytermuxdoc/blob/master/pictures/termux.png)

Termux不完全指南

必读：

命令行的艺术：https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md

Termux官方wiki：https:wiki.termux.com/wiki/Main_Page

[RE:0从零开始的Termux](https://github.com/breathiness/learn-termux)

[+]初始化

1.选择shell

 * bash
![bash](https://github.com/myfreess/Mytermuxdoc/blob/master/pictures/bash.png)
 
bash是Termux及大多数linux发行版的默认shell(但Termux还默认安装了dash)

启动文件为`$PREFIX/etc/bash.bashrc`和`~/.bashrc`

注：这只是一种简单的说法，bash启动文件的详细情况见下文。

 * $PS1

Termux默认只定义了bash提示符为`$`,不包括当前路径，未免不太方便

本人修改bash提示符为['当前路径']，这已经够我用了。

修改方法：`nano ~/.bashrc`，然后修改提示符变量PS1为

```shell
PS1='[\w]'
```

`\w`意为当前路径，必须处于单引号包裹中。

如果想自行配置，有文一篇[用PS1美化你的shell]：https://blog.mycike.com/846.html

 * bash-completion
 
bash不会很漂亮，但补全还是有的。 

安装&使用：

```shell
apt install bash-completion
```

 * zsh
 
安装:

```shell
apt install zsh
```

 * oh-my-zsh
![ohmyzsh](https://github.com/myfreess/Mytermuxdoc/blob/master/pictures/oh-my-zsh.png) 
 
zsh的配置相当麻烦，所以一般使用oh-my-zsh这个一键脚本进行配置
 
termux有一键安装oh-my-zsh的脚本，可以更换终端配色与字体。

安装:

```shell
cd ~
git clone https://github.com/Cabbagec/termux-ohmyzsh
cd termux-ohmyzsh
bash install.sh
```

安装后默认主题是agnoster
![agnoster](https://github.com/myfreess/Mytermuxdoc/blob/master/pictures/agnoster.png)

可以通过编辑`$HOME/.zshrc`中的`ZSH_THEME`来更换主题

* zsh-syntax-highlighting
 
https://github.com/zsh-users/zsh-syntax-highlighting 

默认包含于Cabbagec的脚本中。

* fish
 
安装:`apt install fish`

fish是开箱即用型shell,UI风格对用户非常友好。

如果希望拥有一个更加强大的shell,可以用oh-my-fish和fisherman来安装主题和插件。

```shell
#ohmyfish安装
curl -O https://raw.githubusercontent.com/myfreess/Mytermuxdoc/master/setup/fishsetup.sh
bash fishsetup.sh
```

注：fish不兼容bash的script语法。

 * 冷水
 
1.ohmyzsh可能因为某些因素卡死你的终端！ 

2.fish的补全不区分大小写！

3.oh-my-zsh有机率触发exec()error，一经触发无法修复(别问我为什么这么清楚，这是我换用bash的真正原因)。

注：[Termux Shell列表](https://wiki.termux.com/wiki/Shells) 

注：在Termux的shell会话中可通过输入`exit 0`来关闭一个shell会话，Ctrl-d也可。
 
注：在Termux中长按屏幕左边滑出的'keyboard'可打开一些特殊键
 
 * Termux启动的是登录Shell吗？
 
从'echo $0'的输出来看，bash以非登录shell的模式启动。

但从bash的真正启动文件'$PREFIX/etc/profile'来看，bash以登录模式启动。并且Termux打开时启动的第一个程序是'login'(通过内核调用'exec()'启动)。

注：Termux的bash启动文件'profile'中source了'$PREFIX/etc/bash.bashrc'和'$HOME/.bashrc'，etc目录内的'bash.bashrc'先于home目录内的'.bashrc'被读取。
 
 * Termux上的chsh是什么？
 
Linux上的chsh通过修改`/etc/passwd`文件来改变用户的启动Shell，那Termux这个单用户机制的应用呢？

答案是通过修改`~/.termux/shell`文件，这个文件不存在时Termux就去启动bash，存在时则将其作为启动Shell。

chsh不过一bashscript而己，它会从`$PREFIX/bin`中寻找用户需要的shell，并在`~/.termux`目录中建立名为shell的符号链接指向它。

2.选择文本编辑器

Linux上最流行的编辑器是Vim和Emacs，nano则适用于新手。

但termux上的nano对中文的显示支持有问题，中文字符串会随机缩短。zile更水，居然不支持中文输入……

没奈何，Vim与Emacs取其一。

Vim高效快速，Emacs功能强大且自带帮助手册，自行选择。Neovim作为vim的分支，也作为选择之一。

Termux官方列出了所有Termux上可用的文本编辑器，详情见wiki。

 * 论Emacs的全能性

Emacs在设计之初的目标就是：提供一个完整的系统工具集所应有的功能(从浏览器到游戏)，又因为Emacs Lisp的出现，它可以让用户简单地对它进行扩展。

例如Emacs其实可以煮咖啡(coffee.el)，不是玩笑话，详情可搜索词条"Emacs煮咖啡"。

 * 退出Vim

按下'ESC'键，进入命令模式，输入':wq'将保存文件并退出Vim。 

 * vim中文支持
 
```shell
mkdir ~/.vim
cd ~/.vim
touch vimrc
#以下为vimrc内容
set fileencodings=utf-8,gb2312,gb18030,gbk,ucs-bom,cp936,latin1
set enc=utf8
set fencs=utf8,gbk,gb2312,gb18030
#保存文件
#在shell的init文件中加一行
source ~/.vim/vimrc
#结束
```

注：google中文输入法无法在termux内输入中文。

注：使用Vim或Emacs时按手机返回键，点按呼出键盘将不会生效，需点按屏幕左边滑出的'keyboard'按钮。

尽管vim老手从不用上下左右键，但我等新手还是少不了它的。

酷安某用户修改了termux的特殊键，增加了上下左右键，修改版apk我放在仓库里了，有需要自己下。

后注：termux更新后增加了juicessh风格的特殊键，修改版已从仓库中移除。

 * 横屏编辑
 
除非使用平板，否则很难兼顾特殊键与中文。 

3.选择软件源

termux的官方源软件包齐全，但没有打包好的python和ruby依赖包。像pynacl这类包含c库依赖的模块一般无法正常安装。同时官方源不保留旧版本软件包。想安旧版可以试试`Tuna`，这是由中国某大学维护的软件源，支持Termux。还有由xeffyr维护的Termux Mirror。某些源包含openjdk这类官方源中不包含的软件包(Extra源)。在`$PREFIX/etc/apt/source.list`加入一行`deb https://termux.xeffyr.ml/ extra main x11`即可。

简单操作一下：

```shell
apt-key adv --keyserver pool.sks-keyservers.net --recv 9D6D488416B493F0
echo 'deb https://termux.xeffyr.ml/ extra main x11' >> $PREFIX/etc/apt/source.list
```

 * 注：extra源的编译脚本与补丁由github用户xeffyr提供。

通过更换第三方源甚至可以安装metasploit，在官方wiki可见。

https://github.com/termux/termux-root-packages 这个仓库内有编译libusb,aircrack-ng,tcpdump的脚本。

同时termux官方给出了一个python脚本，帮助用户构建自己的deb包。

一键配置所有第三方源:`curl -O https://raw.githubusercontent.com/myfreess/Mytermuxdoc/master/setup/sources.sh`

 * Termux第三方源列表

[1]Tuna镜像源

[2]由xeffyr维护的旧版本源

[3]由xeffyr维护的Extra源

[4]由Auxilus维护的metasploit源

[5]由Grimler91维护的rootpackage源

[6]its-pointless

 * 后缀为dev的deb包是什么？

pkg-config文件与include文件。

 * termux特色package

包括termux-tools,termux-exec,tsu等

经检查，除termux-exec，termux-elf-cleaner使用C语言编写，其他大部分为bashscript，不得不让人感叹Linux的高度可定制性！

下面给出列表。

termux-tools:bashscript工具集，包含`termux-fix-shebang termux-info termux-open termux-open-url termux-reload-settings termux-setup-storage termux-wake-lock termux-wake-unlock`

termux-api:与Android交互需要使用的工具集，powered by bashscript。

termux-apt-repo:

termux-create-package:字面意思

termux-am:apk文件，在`$PREFIX/libexec/termux-am`目录下。是am命令的Android8.0特别版。

[附录]am命令

am全称activity manager，你能使用am去模拟各种系统的行为，例如去启动一个activity，强制停止进程，发送广播进程，修改设备屏幕属性等等。

就是Android上的应用调试器，给开发者用的。

termux-elf-cleaner：elf文件处理工具

termux-exec：下面介绍

 * 手动安装deb包
 
dpkg -i ./xxx.deb 

4.软件包管理

termux自带apt，基于apt封装了一个pkg命令

`apt install $package` 或 `pkg install $package` 是一样的。

但我建议使用apt，这样做可以让你在将来快速适应deb系的发行版。

 * apt update
 
更新deb包索引信息，当初次打开无法安装软件包时可运行此命令。

 * apt下载的deb包在哪儿？
 
`$PREFIX/var/cache/apt/archives`

 * apt的本质
 
基于dpkg封装的自动化包管理器。

 * 为什么不能兼容debian系发行版的deb包(那怕架构相同)？
 
因为软件的安装目录是在deb包中被预制定好的，并且以绝对路径形式制定。

 
###########

[+]有关termux的文章链接

http://www.freebuf.com/geek/170510.html

http://www.sqlsec.com/2018/05/termux.html

###########

>前言：

我希望我将来可以达到下图的水准

![mydream](https://github.com/myfreess/Mytermuxdoc/blob/master/pictures/mydream.png)

不过现实还差得远呢。

$TERMUX_PACKAGE

termux可安装的linux应用介绍。

 * tsu
 
tsu是Termux独有的su程序，允许用户以root权限运行Termux内的linux应用。

使用：

```shell
apt install tsu
tsu
#exit可退出root用户状态
#单次使用root权限
tsudo command
```
 
 * lftp
 
轻量的cliftp客户端。
 
 * openssh
 
包含ssh,scp,sftp,sshd,sftpd，ssh-keygen等多个程序。

sshd是ssh服务守护进程，ssh则是客户端。

Termux的sshd默认端口为8022，不支持密码登录，必须将你自己的ssh公钥输出到`~/.ssh/authorized_keys`这个文件内。

如要在外部网络访问可以使用frp和ngrok将你的Server端口映射到公网上，如梯子钱充够了也可按官方教程使用tor进行端口映射。

 * proot

proot是`chroot``mount --bind`的用户空间实现，一个简单的容器应用。

termux的proot包内还包含了一个名为`termux-chroot`的bashscript，可在Termux内模拟linux目录结构，外加模拟root权限。

proot使用帮助：https://github.com/myfreess/Mytermuxdoc/blob/master/HowToUseProot.md

顺便一说，github上的termux用户们热心地编写了一系列用于安装运行发行版的bashscript。

可选的发行版：

Fedora,kali,Arch,debian，ubuntu，alpine……

安装：

官方建议见此处:https://wiki.termux.com/wiki/PRoot

隔壁群群主的脚本:https://github.com/YadominJinta/atilo ，可以快速安装linux发行版。

选择发行版:

桌面环境：


 * curl & wget 
 
普通的下载器(-_-)……

言归正传，curl支持的传输协议其实不少，以下为官方介绍:

![curl](https://github.com/myfreess/Mytermuxdoc/blob/master/pictures/curl.svg)

>A command line tool and library for transferring data with URL syntax, supporting HTTP, HTTPS, FTP, FTPS, GOPHER, TFTP, SCP, SFTP, SMB, TELNET, DICT, LDAP, LDAPS, FILE, IMAP, SMTP, POP3, RTSP and RTMP. libcurl offers a myriad of powerful features https://curl.haxx.se/

wget就略单薄一点，只有http和ftp(加上ssl)支持。

它们的下载线程是单线程机制的，所以效率上肯定比不上aria2，优势在于它们是web调(ri)试(zhan)与自动化任务的一把好手。

例如在美剧Mr.Robot中，主角用wget拿了个服务器低权限shell，然后开始提权&搞破坏……

 * aria2
 
强悍的cli下载器，支持Metalink等现代下载技术。 

开启rpc配合transdroid食用更佳。

安装&开启rpc

```shell
apt install aria2
#开启rpc
aria2c --enable-rpc --rpc-listen-all
```
然后可以用transdroid方便地从127.0.0.1:6800连接了，下载奇快！

 * weechat&irssi
 
Irc聊天用 
 
 * nginx
 
高性能http服务器。 

>默认的普通权限无法启动 nginx, 需要模拟root权限才可以

运行：

```shell
apt install proot nginx -y
termux-chroot
nginx
```
默认端口8080。

 * apache2
 
```shell
apt install apache2
apache2ctl start
```
 
 * tor
 
tor网络客户端。 
 
 * mutt
 
邮件客户端。 
 
 * linuxutil&busybox&coreutil
 
包含whatis，netcat等实用linux小工具 

 * game
 
gnuchess,gnugo啥的。

extra源有doxbox,stable源有fontz，玩些字符游戏没问题！

apt search game可以找到更多游戏！

$TERMUX_COMMON

Termux日常使用帮助。

>注:尽管Termux官方尽可能的为Termux营造linux的使用感受，但仍与linux有很大不同。此帮助手册包含大量linux发行版不支持的Termux特殊性质与配置。
 
[+]justforfun

```shell
curl wttr.in?lang=zh
#天气预报
echo "https://github.com/myfreess" | curl -F-=\<- qrenco.de
#终端下的二维码
```

[+]拉取并使用源码文件

https://github.com/myfreess/Mytermuxdoc/blob/master/repo.md

[+]目录结构

termux根目录为`/data/data/com.termux/files`

内有home和usr两个目录，对应的变量是`$HOME`和`$PREFIX`。

如果觉得这样的目录结构不习惯，可以执行

```shell
apt install proot -y
termux-chroot
```

但这对性能有一定损伤。

[+]获得读写sdcard权限

`termux-setup-storage`获得sdcard读写权限。

`ln -s target linkname`建立软链方便管理。

[+]启动效果

一般打开Termux时会显示`$PREFIX/etc/motd`中的文字，可自行更改。

也有很多人在bashrc中使用figlet等工具制作启动时的艺术字效果。

[+]配色方案和字体

将字体文件复制到$HOME/.termux内，重命名为font.ttf，重启生效。

配色方案如法炮制，复制后重命名为colors.properties。

[+]More

Termux中长按屏幕，点按'More'出现更多选项。

[+]在Android中调用Termux编辑器

```shell
cd ~
ln -s /sdcard/documents downloads
mkdir ~/bin
cd ~/bin
ln -s $PREFIX/bin/nano termux-file-editor
```

我用nano，vim用户与Emacs用户可如法炮制。

完成后在Termux的home目录内生成downloads目录，编辑完成后文件将保存在这个目录。

[+]url钩子

建立文件`~/bin/termux-url-opener`，然后可以在Android中用Termux打开链接。

可以在此处链接you-get或lynx，自已写一个处理url的脚本也可以。

[+]文件传输

1.文件流向:Termux~>client

```shell
cd $dir
python -m SimpleHTTPServer
```
python将监听8000端口，并开启一个简单的http服务器，根目录为$dir，这相当方便易用，风险也小(拒绝使用登录式ftp)

当然了，如果你要的只是快，这样的确很好。但这样不方便整个目录的复制，如果你经常传输文件，可以考虑一下pure-ftpd

pure-ftpd的使用此处不作介绍，以下是一些ftpserver的注意事项。

1.不要为ftp用户开启可写权限

2.只使用匿名登录

如果需要修改、上传文件，那还是用sftp吧。

[+]改造termux
 
注：需要提前学习一些Linux知识，可以先用proot运行的发行版练练手。

前面提到了termux上一些特色package其实是bashscript，那么，可以通过自己编写脚本来实现一些自定义功能

例如，为bash上一把密码锁。

现成的实现在此https://github.com/myfreess/termux-bashlock

[+]与Android交互

这可能是Termux最强大的功能，需要以下支持:

 * [Termux:API]Termux扩展包。
 
 * [termux-api]用bashscript编写的，方便用户使用的工具集。
 
其实Android应用一直以来都支持命令行交互，你可以不用默认工具集，自己重新编写脚本来实现短信发送一类的功能。但这样做并不方便，最好还是在用户脚本内调用这些工具集完成所需功能。

目前已经有人实现了基于短信的交互式shell，异常强大！

注:用了修改版Termux就用不了这个扩展包了，因为Apk签名不统一。

[附录]Neoterm

Termux分支，特点是兼容Google中文输入法，自带oh-my-zsh安装脚本，Adb与Fastboot,以及强到爆炸的……

Xorg及图形环境。

目前测试版已经有图形会话选项了，但尚不稳定。

[附录]Termux原生图形界面

见extra源，xorg还有openbox啥的。

有文一篇https://yadominjinta.github.io/2018/07/30/GUI-on-termux.html








 
 
 
 
 
 
 
 
 
 
 
 
 