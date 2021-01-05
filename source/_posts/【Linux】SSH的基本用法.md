---
title: 【Linux】SSH的基本使用
categories:
  - Linux
tags:
  - Linux
  - Arch
date: 2020-12-31
---

## 基础
在 Linux 系统上 SSH 是非常常用的工具，通过 SSH Client 我们可以连接到运行了 SSH Server 的远程机器上。SSH Client 的基本使用方法是：
```
ssh user@remote -p port
```
- user 是你在远程机器上的用户名，如果不指定的话默认为当前用户  
- remote 是远程机器的地址，可以是 IP，域名，或者是后面会提到的别名  
- port 是 SSH Server 监听的端口，如果不指定的话就为默认值 22 
 
在执行了 ssh 命令之后，远程机器会询问你的密码。在输入密码的时候，屏幕上不会显示明文密码，也不会显示 ******，这样别人就不会看到你的密码长度了，按下回车即可登入。

登入之后，就可以远程控制机器啦！

## 安装OpenSSH Server
```
local$ ssh user@remote -p port 
ssh: connect to host remote port 22: Connection refused
```
如果你遇到了上面的消息，说明在远程机器上没有安装 SSH Server，特别地，如果远程机器运行的是 Ubuntu Desktop 系统，那么默认是没有安装 SSH Server 的。这个时候，你可以联系管理员让他安装 SSH Server，或者如果你有 sudo 权限的话，可以执行下面命令安装：
```
sudo apt install openssh-server
```
相应的本地机器需要执行下面的命令：
```
sudo apt install openssh-client
```
## 免密码登入

在本地机器输入下面指令生成 SSH 钥匙，一路回车即可。
```
ssh-keygen
```
输出的内容告诉了我们，生成的公钥放在了 ~/.ssh/id_rsa.pub，私钥放在了 ~/.ssh/id_rsa。接下来，我们要让远程机器记住我们的公钥。最简单的方法是
```
ssh-copy-id user@remote -p port
```

## 配置别名

每次都输入 ssh user@remote -p port，时间久了也会觉得很麻烦，特别是当 user, remote 和 port 都得输入，而且还不好记忆的时候。配置别名可以让我们进一步偷懒。

比如我想用 ssh lab 来替代上面这么一长串，那么在 ~/.ssh/config 里面追加以下内容：
```
Host lab
    HostName remote
    User user
    Port port
```
保存之后，即可用 ssh lab 登入，如果还配置了公钥登入，那就连密码都不用输入了。

## 传输文件

在两台机之间传输文件可以用 scp，它的地址格式与 ssh 基本相同，都是可以省略用户名和端口，稍微的差别在与指定端口时用的是大写的 -P 而不是小写的。不过，如果你已经配置了别名，那么这都不重要，因为 scp 也支持直接用别名。scp 用起来很简单，看看下面的例子就明白了：
```
# 把本地的 /path/to/local/file 文件传输到远程的 /path/to/remote/file
scp -P port /path/to/local/file user@remote:/path/to/remote/file

# 也可以使用别名
scp /path/to/local/file lab:/path/to/remote/file

# 把远程的 /path/to/remote/file 下载到本地的 /path/to/local/file
scp lab:/path/to/remote/file /path/to/local/file

# 远程的默认路径是家目录
# 下面命令把当前目录下的 file 传到远程的 ~/dir/file
scp file lab:dir/file

# 加上 -r 命令可以传送文件夹
# 下面命令可以把当前目录下的 dir 文件夹传到远程的家目录下
scp -r dir lab:

# 别忘了 . 可以用来指代当前目录
# 下面命令可以把远程的 ~/dir 目录下载到当前目录里面
scp -r lab:dir/ .
```

## 保持程序在后台运行
有时候你想要在远程的机器上跑一个需要长时间运行的程序，比如一些计算，然后当你睡了一觉再登入远程的机子上却发现什么结果都没有。这是因为一旦 ssh 进程退出，所有它之前启动的程序都会被杀死。那么有什么办法可以保持程序在后台运行呢？

你需要在远程的机子上使用 tmux。tmux 是一个会话管理程序，他会保持程序一直运行着。在 Ubuntu 上你可以通过 sudo apt-get install tmux 来安装。
```
remote$ tmux
```
这样你就进入到了 tmux 管理的会话中，之后你再运行任何东西都不会因为你退出 ssh 而被杀死。要暂时离开这个会话，可以先按下 ctrl+b 再按下 d。要恢复之前的会话，只需要执行
```
remote$ tmux attach
```
tmux 还能管理多个窗口、水平竖直切分、复制粘贴等等，你可以看看[这篇不错的文章](https://link.zhihu.com/?target=http%3A//blog.jobbole.com/87584/)来入门。

如果你是Mac用户，那么十分幸运的是，你几乎不需要任何学习，只要把你的终端由系统自带的 Terminal 换成 iTerm 2。iTerm 2 自带超好的 tmux 支持，你可以像操作本机的标签页一样操作 tmux 会话。你只需要在新建会话的时候使用 tmux -CC，在恢复的时候使用 tmux -CC attach 即可。具体的可以参见 iTerm2 and tmux Integration。

最后强调一遍，tmux 应该运行在远程的机子上，而不是本机上，否则程序在 ssh 退出时依然会被杀死。


> 本文参考：[SSH 基本用法](https://zhuanlan.zhihu.com/p/21999778) 