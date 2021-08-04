---
title: 当你拿到一台崭新的 Mac 电脑时，我们应该如何快速高效配置开发环境？
date: 2021-08-04 14:01:21
tags:
- 高效方法
categories: 高效方法
description: 当你拿到一台崭新的 Mac 电脑做为工作开发电脑时，需要从 0 搭建开发环境。本文记录了工作中如何在 Mac 电脑上快速配置环境，同时安利一些高效的开发工具和搭建指南。
---

# 安装包管理器

## HomeBrew

最重要的一个工具，没有之一，有了这个，后面的涉及到的工具才方便安装，必须想办法安装成功！

```css
Homebrew是一款Mac OS平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。
```
安装命令：
```groovy
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

如果半天下载 不下来，不用等了，直接搜国内的镜像源，分分钟下载下来。

直接切换，用以下方式：

```groovy
/usr/bin/ruby -e "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install)"
```

可参考：

[Homebrew是什么？以及使用](https://www.jianshu.com/p/f4c9cf0733ea)

[HomebrewCN：Homebrew的国内安装脚本，从此告别龟速更新](https://zhuanlan.zhihu.com/p/146001246)(亲测很快)

[mac下国内安装Homebrew教程](https://www.cnblogs.com/xibushijie/p/13335988.html)

[Mac必备神器Homebrew](https://zhuanlan.zhihu.com/p/59805070)

# 终端

## iTerm2

取代系统默认的命令，效率大大提升。同时可以直接设置为系统默认终端。在Android Studio操作同样高效。

```groovy
brew cask install iterm2
```

https://sourabhbajaj.com/mac-setup/iTerm/

## zsh

- [终极 Shell——ZSH](https://zhuanlan.zhihu.com/p/19556676)
- [终端折腾记](https://g2ex.github.io/2018/01/21/linux-terminal-zsh-notes/)
- [zsh 全程指南](https://wdxtub.com/2016/02/18/oh-my-zsh/)
- [zsh oh-my-zsh 插件推荐](https://hufangyun.com/2017/zsh-plugin/)
- [Show Off your Config | powerlevel9k](https://github.com/bhilburn/powerlevel9k/wiki/Show-Off-Your-Config)

**安装插件**

```groovy
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
  
# ~/.zshrc
plugins=(
  git
  autojump
  extract
  osx
  z
  zsh-autosuggestions
  zsh-syntax-highlighting
)
```

此时，需要研究一下 `vim`相关的，在mac中操作少不了的。后面会提到。

# Git

https://git-scm.com/book/en/v1/Getting-Started-Installing-Git

```groovy
$ brew install git
$ git config -e --global
```

生成 ssh key ,配置到 gitlab

```groovy
color.ui=auto
user.name=JuneYang
user.email=JuneYang@qq.com
core.editor=vim
alias.co=checkout
alias.ci=commit
alias.st=status
alias.br=branch
alias.hist=log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short
alias.changelog=log --pretty=format:%s
alias.type=cat-file -t
alias.dump=cat-file -p
reviewboard.url=http://xx.com/
hbt.dir=~/project/build_scriptxx/source/BuildScript/code_analysis
jira.user=JuneYang@qq.com
jira.pwd=xxx
```

参考 [First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

# Sdkman

安装

```groovy
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

安装 SDKMAN

```groovy
curl -s "https://get.sdkman.io" | bash
```

安装 Java

```groovy
sdk install java 8.0.265.hs-adp
```

如果没有版本，请根据提示选择相应的版本即可。

安装 Kotlin

```groovy
sdk install kotlin
```

安装 groovy

```groovy
sdk install groovy
```

安装 Gradle

```groovy
sdk install gradle
```

安装 Maven

```groovy
sdk install maven
```

参考：

[sdkman－管理sdk神器](https://zhuanlan.zhihu.com/p/82315812)

## Java

```
brew tap adoptopenjdk/openjdk
brew cask install adoptopenjdk8
brew cask install adoptopenjdk9
brew cask install adoptopenjdk10
brew cask install adoptopenjdk11
```

目前使用 jdk8

https://github.com/AdoptOpenJDK/homebrew-openjdk

个别安装太慢，这个后面看看有没有其他更快安装的办法，求告知。

## Python

```groovy
brew install python@2
```

# nodejs

推荐使用 nvm

https://github.com/nvm-sh/nvm

# 输入法

- 百度
- 搜狗五笔
- ……

根据个人喜好安装，对于追求效率的人来说，我选择的是搜狗五笔，打字时字体调到最大的字体，别再好奇的问，你的输入法怎么这么大，我只能说，你打字时在看哪里，专注打字，大就对了。

# Utility

```
brew install Alfred 
#Alfred是一个让你可以丢掉鼠标的神器。Alfred 是 Mac 系统上一款专注于效率提升的著名应用，它能帮你快速打开网页、快速进行自定义搜索、查看剪贴板历史、快速查询单词等等
brew install wget # 命令行下载工具
brew install htop # 命令行任务管理器
brew install ffmpeg # 多媒体工具包
brew cask install the-unarchiver # 压缩工具
brew cask install iina # 多媒体播放器 iina
brew cask install spectacle # 窗口管理工具
brew cask install sonic-visualiser # 音频分析软件
brew cask install hex-fiend # 二进制文件查看器
brew cask install lunar # 显示器亮度调节 https://lunar.fyi/

```

# IDE

## Android Studio

```groovy
brew cask install android-studio
```

- [Android Studio download archives](https://developer.android.com/studio/archive)

## Plugins

- http://plugins.jetbrains.com/plugin/7380-adb-idea
- http://plugins.jetbrains.com/plugin/1065-checkstyle-idea

## IDEA Community

```groovy
brew cask install intellij-idea-ce
```

直接通过命令安装，省去到处找安装包的麻烦。

# 编辑器

## Atom

```
brew cask install atom
```

## VS Code

```
brew cask install visual-studio-code
```

## Vim

```groovy
brew install vim
```

补充常用操作:

```cmake
如果是vim，则：Esc 退出编辑模式，输入以下命令： 

:wq  保存后退出vi，若为 :wq! 则为强制储存后退出（常用） 

:w    保存但不退出（常用） 

:w!   若文件属性为『只读』时，强制写入该档案 

:q    离开 vi （常用） 

:q!   若曾修改过档案，又不想储存，使用 ! 为强制离开不储存档案。 

:e!   将档案还原到最原始的状态！ 
```

# 文档查看器

## Dash

```
# latest
brew cask install dash

# 4.6.7 with license
brew cask install https://raw.githubusercontent.com/Homebrew/homebrew-cask/baf4f35e70c225fe1a8a60ec3b4e22604187238d/Casks/dash.rb
```

- [Dash for macOS - API Documentation Browser, Snippet Manager - Kapeli](https://kapeli.com/dash)
- [Mac 软件使用之 Dash](https://www.jianshu.com/p/bc90b212c0a7)
- [Offline documentation browser inspired by Dash](https://zealdocs.org/)
- [Downgrade brew cask application](https://www.jverdeyen.be/mac/downgrade-brew-cask-application/)

## [DevDocs.io](http://devdocs.io/)

- https://github.com/egoist/devdocs-desktop
- https://github.com/ragingwind/devdogs

## Zeal

- https://github.com/zealdocs/zeal
- [Build Zeal on macOS](http://xavieryao.github.io/article/zeal)
- [Build Zeal for Mac OS X](https://mazhuang.org/2016/01/16/build-zeal-for-mac-osx/)

# 反编译工具

## APKTOOL

```
brew install apktool
```

## JADX

```
brew install jadx
```

## JD-GUI

```
brew cask install jd-gui
```

# 密码管理

## LastPass

```
brew cask install lastpass
brew install lastpass-cli
```

# 文件比较

## Meld

- http://meldmerge.org/
- Meld 下载 https://github.com/yousseb/meld/releases/

# FTP Client

## FileZilla

```
$ brew cask install filezilla
```

- http://macappstore.org/filezilla/
- https://filezilla-project.org/download.php?platform=osx

# SSH Client

## Termius

http://www.termius.com/

# File Manager

- https://www.trankynam.com/xtrafinder/
- http://macappstore.org/xtrafinder/

# Proxy Server

## mitproxy

## charles

# VPN

EasyConnect

# 其它文件系统支持

## Linux Ext-4

https://www.paragon-software.com/home/extfs-mac/

## Windows NTFS

https://www.paragon-software.com/home/ntfs-mac/

# 其他项目相关

## 安装Repo

```groovy
# Debian/Ubuntu.
$ sudo apt-get install repo

$ mkdir -p ~/.bin
$ PATH="${HOME}/.bin:${PATH}"
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo
$ chmod a+rx ~/.bin/repo
```

前提：apt-get需要安装好，如果没有安装好，用如下命令:

```groovy
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

brew install libxml2
```

如果出现错误：

## 安装apt

[mac 安装apt](https://www.jianshu.com/p/7330d3b6d561)

注意解压时，如果用命令无法解压，直接双击解压即可。

```groovy
tar -xvf fink-0.43.0.tar.gz or tar -xvf fink-0.43.0.tar
```

此处更新版本后才能执行下去。

https://www.finkproject.org/download/srcdist.php

下载最新版本 `flink-0.45.2.tar.gz`解压后执行

```
cd fink-0.45.2
sudo ./bootstrap
```

其中的选择选择提示的默认选项即可。下载时间10分钟左右不等。

```undefined
/sw/bin/pathsetup.sh
fink selfupdate-rsync
fink index -f
```

在执行上面三个命令时 竟然提示无此命令，甚至我试ls vim都不好用，吓我一跳，最终无法重启控制台后好用，虚惊一场。

到此 安装完成 `sudo apt-get` 可以正常使用

```groovy
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```

## repo的使用

```groovy
repo init -u "xxx仓库地址"

repo sync
repo start master --all
```

repo需要配置在path中

```groovy
vim ~/.bash_profile
```

之后进入编辑模式

第一步：

> vim ~/.bash_profile

第二步：

> E->I->Enter

输入：

```groovy
export PATH =~/bin:$PATH
```

最后：

> esc-> :wq 保存 然后生效。

注意：有时会失效，原因未找到。关注中。。。

可以 暂时这样：配置为临时环境变量(也可配置为永久的)

```groovy
PATH=~/bin:$PATH
```

之后 可直接访问.

```groovy
//repo init -u "xxx"
 
repo sync
repo start master --all
```

参考：

[MAC系统下的repo环境搭建以及下载更新](https://blog.csdn.net/leehs334/article/details/105101574)

- 生成秘钥对

ssh-keygen -t rsa -C  "juneyang@xx.com"

一路enter，不输入其他信息。为空处理。

添加repo到系统目录下

```groovy
PATH=~/bin:$PATH
```

查看PATH路径

```groovy
echo $PATH
```

之后可以直接 调用

```groovy
repo init
```

## 问题记录

Android Studio中导入项目：

1. 调试安装遇到的问题installation failed with message INSTALL_FAILED_SHARED_USER_INCOMPA

   > 去掉AndroidManifest.xml文件中android:sharedUserId

2. 当第一个repo仓库成功后，新建第二个仓库时出现问题

   > 执行repo init时候报错“error: manifest missing or unreadable -- please run init”

**解决办法：**

删除 当前文件下中隐藏的 `manifests.git` ，之后重新执行`repo init`。

关于如何查看隐藏的文件见下文。



# 其他Mac操作相关

1. 删除文件

   > 小米键盘：win+BackSpace

2. 外接键盘右键功能

   > Ctrl+鼠标右键

3. 如何显示隐藏的文件

   > Command+Shift+. 可以显示隐藏文件、文件夹，再按一次，恢复隐藏；
   > finder下使用Command+Shift+G 可以前往任何文件夹，包括隐藏文件夹。

4. adb 环境配置

   [解决Mac上adb: command not found问题](https://blog.csdn.net/weixin_38327562/article/details/71057276?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

   [mac zsh 配置 adb 环境变量 “zsh: command not found: adb”](https://blog.csdn.net/qq_35684085/article/details/104743835?utm_medium=distribute.pc_relevant.none-task-blog-title-2&spm=1001.2101.3001.4242)

   打开 iTerm 终端依次输入下面命令:

```groovy
echo 'export ANDROID_HOME=/Users/$USER/Library/Android/sdk' >> ~/.zshrc
echo 'export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools' >> ~/.zshrc
```

然后保存设置：(更新设置)

```groovy
source ~/.zshrc
```

5. zsh命令安装后的问题

[更新完 zsh 说我目录权限问题的解决](https://www.jianshu.com/p/03bcc3e2930e)

6. 设置iTerm2为默认的term

   这个很简单就iTerm2->Make iTerm2 Default Term就好了.Android Studio中重启生效。

# MAC快捷键

## CheatSheet

安装这个后，在相应的软件界面，长按`command`，就会出现当前界面的所有快捷键，你值得拥有。

## 系统

- 切换同一个应用的多个窗口 `Cmd + ~`
- 锁定屏幕 `Ctrl + Cmd + Q`
- 窗口预览 `Ctrl + Up Arrow`
- 任务管理器 `Ctrl + Alt + Esc`
- 截图 `Cmd + Shift + 4`

## 浏览器

- 浏览器刷新 `F5 -> Cmd + R`
- 光标切换到地址栏 `Alt + D -> Cmd + L`
- 返回上一页 `BackSpace -> Cmd + Left Arrow` / `Cmd + [`
- 滚动到底部/顶部 `Cmd + Down Arrow` / `Cmd + Up Arrow`
- [Chrome keyboard shortcuts](https://support.google.com/chrome/answer/157179?hl=en)
## 启动台设置
[给Mac的启动台Launchpad设置快捷键](https://www.xia1ge.com/launchpad-hot-key.html)

未完待续……

参考：

- [Mac系统如何显示隐藏文件](https://www.zhihu.com/question/24635640)

- [Repo命令记录](https://juejin.im/post/6844904125977608199)

- [反思｜Android源码模块化管理工具Repo分析](https://juejin.im/post/6844904148102545416)

- [Mac 入门 一 基本用法](https://www.cnblogs.com/TankXiao/p/2845413.html)
- [将多个显示器与 Mac 配合使用](https://support.apple.com/zh-cn/ht202351)
- [Mac 键盘快捷键](https://support.apple.com/zh-cn/HT201236)
- [你可能不知道的 Mac 技巧 - 文本操作](http://singsing.io/blog/mac/Mac-1/)
- [你可能不知道的 Mac 技巧 - 截图，Gif 制作及 App 推荐](http://singsing.io/blog/mac/Mac-2/)
- [你可能不知道的 Mac 技巧 - macOS 的窗口、标签与工作区](http://singsing.io/blog/mac/Mac-3/)
- [从 Windows 到 OS X，新手入门解惑 | Hello Mac](https://sspai.com/post/30981)
- [Mac 开发配置手册](https://aaaaaashu.gitbooks.io/mac-dev-setup/content/Apps/index.html)
- [Mac新手入门以及常用软件推荐](https://wsgzao.github.io/post/mac/)
- [mac新手指南-基础环境搭建篇](http://www.zorin.xin/mac/2017/11/20/mac新手指南-基础环境搭建篇.html)
- [一直使用 Windows 的人如何尽快熟练使用 OS X？](https://www.zhihu.com/question/20386836)
- [你不应错过的 22 款 Mac 效率工具 App | 2015 年度盘点](https://sspai.com/post/32453)