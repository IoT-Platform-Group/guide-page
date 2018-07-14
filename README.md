# 物联网开发小组帮助面板
## 环境配置类

### windows工作环境下工具的安装和相关语言环境的配置

#### PyCharm(python)

首先需要从python官网安装python（本人使用的版本是python3.6.4），官网下载页面：https://www.python.org/downloads/ (值得注意的是，官网直接下载的速度较慢，有条件的话推荐使用迅雷等下载工具)

之后就是安装PyCharmIDE，官网链接如下：http://www.jetbrains.com/pycharm/download/#section=windows (如果已经申请了学生免费的话，可以直接下载Pro版本)

#### IDEA(java)

首先需要安装java环境（推荐使用oracle官方的jdk1.8版本）

具体教程如下（面向jdk1.8版本）：https://jingyan.baidu.com/article/6b97984dd257b41ca2b0bf86.html

jdk官方下载链接：http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

#### CLion(C/C++)

首先需要安装MinGW以配置C环境，然后从官网安装CLionIDE

具体教材如下：http://www.hankcs.com/program/cpp/jetbrains-clion.html



### vagrant虚拟机配置

关于vagrant环境的配置，可以参照网上的教程（教程地址：https://www.cnblogs.com/alexyang8/p/3380936.html，笔者本人的博客地址：https://www.cnblogs.com/HansBug/p/7403306.html）

在选择镜像的时候，可以在官方网站上搜寻想要的镜像（官网镜像搜索地址：https://app.vagrantup.com/boxes/search）。

在选择镜像的时候，有几点建议：

* 注意看系统的版本，确定是自己最需要的版本
* 注意看镜像所支持的虚拟机版本（virtualbox or vmware，不过推荐使用vmware的镜像）
* 注意镜像的最近维护时间（即右边的Released项），建议优先选择近期仍在维护的（近期依然在不断更新的镜像在后期环境配置的时候会少很多不必要的麻烦）

此外，笔者选择的镜像版本是：https://app.vagrantup.com/ubuntu/boxes/xenial64 （ubuntu16.04，7.13日仍在维护，且笔者实测使用起来体验很不错），centos系统推荐：https://app.vagrantup.com/albmtez/boxes/centos7-x64（centos7，也是近几天仍在维护）



### ubuntu16环境入手

#### 更换apt镜像源

如果你使用的是vagrant虚拟机的话，那么一开始你的镜像源将是ubuntu的官方镜像源。类似这样

```c
## Note, this file is written by cloud-init on first boot of an instance
## modifications made here will not survive a re-bundle.
## if you wish to make changes you can:
## a.) add 'apt_preserve_sources_list: true' to /etc/cloud/cloud.cfg
##     or do the same in user-data
## b.) add sources in /etc/apt/sources.list.d
## c.) make changes to template file /etc/cloud/templates/sources.list.tmpl

# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
# newer versions of the distribution.
deb http://archive.ubuntu.com/ubuntu xenial main restricted
deb-src http://archive.ubuntu.com/ubuntu xenial main restricted

## Major bug fix updates produced after the final release of the
## distribution.
deb http://archive.ubuntu.com/ubuntu xenial-updates main restricted
deb-src http://archive.ubuntu.com/ubuntu xenial-updates main restricted

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team. Also, please note that software in universe WILL NOT receive any
## review or updates from the Ubuntu security team.
deb http://archive.ubuntu.com/ubuntu xenial universe
deb-src http://archive.ubuntu.com/ubuntu xenial universe
deb http://archive.ubuntu.com/ubuntu xenial-updates universe
deb-src http://archive.ubuntu.com/ubuntu xenial-updates universe

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team, and may not be under a free licence. Please satisfy yourself as to
## your rights to use the software. Also, please note that software in
## multiverse WILL NOT receive any review or updates from the Ubuntu
## security team.
deb http://archive.ubuntu.com/ubuntu xenial multiverse
deb-src http://archive.ubuntu.com/ubuntu xenial multiverse
deb http://archive.ubuntu.com/ubuntu xenial-updates multiverse
deb-src http://archive.ubuntu.com/ubuntu xenial-updates multiverse
```

该镜像源位于海外服务器，由于一些XXX的原因，速度较慢。

所以一般采取的方式是替换为清华大学的国内镜像源，具体做法是讲`/etc/apt/sources.list`文件替换为（需要sudo权限）

```c
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
```

替换完毕后，运行

```shell
sudo apt-get update
```

再运行

```bash
sudo apt-get upgrade
```

以完全更新镜像源

#### 更换python镜像源

和apt镜像源类似，python的镜像源也会存在类似的问题，所以我们采用的方式还是设置为国内镜像源（清华大学镜像源，同步频率大概5min一次，基本可以视为同步）

具体操作是，将`~/.pip/pip.conf`内容替换为（如果文件或路径不存在则手动创建）

```c
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```



### ubuntu16内java环境配置

**JAVA采用oracle官方提供的java8**（而不是OpenJDK，这一点需要注意）

一种安装方式是用和windows类似的下载+配置环境法，不过这边更推荐一种更快速的方法——直接通过添加PPA源，使用apt-get安装。

教材地址如下：https://www.cnblogs.com/iban/p/5540854.html

手动安装教材如下：https://www.cnblogs.com/ccskun/p/5534757.html
