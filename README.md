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
* 注意看镜像所支持的虚拟机版本（virtualbox or vmware，不过推荐使用virtualbox的镜像）
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



## 实际操作类

### Mavan相关实践

Maven是一个快速管理Java工程和工程相关依赖的工具。

在IDEA里，我们可以免安装快速使用maven来对项目进行依赖管理

不仅如此，Maven有相当完整的线上生态圈。

Maven相关基础教程可以直接百度或者看之前写过的实例即可。

Maven搜索页面

* 官方版（**需要翻墙**，需要记录下`groupId`、`artifactId`和`version`并手动配置xml）：http://search.maven.org/
* 另一个搜索页面（不需要翻墙，且用户体验更好，**但是部分包会搜不到**）：http://mvnrepository.com/



### californium实践

#### 入门实践（简单server、简单client、简单上行请求）

1. CoAP测试服务器 [wsncoap.org](http://wsncoap.org)

   【重要】**Coap协议介绍，及其开源实现Californium实战**：https://blog.csdn.net/ty497122758/article/details/77387861

   californium 框架设计分析：https://blog.csdn.net/t91zzh5f/article/details/57145167

2. 开源实现

   维基百科：http://en.wikipedia.org/wiki/Constrained_Application_Protocol

   其中两个开源版本：libcoap（C语言实现）和Californium（java语言实现），比较实用。



### spring-boot实践

spring-boot是一个在java上快速构建MVC服务端的依赖，笔者实测，非常简单易用。

spring-boot快速入手教程（直接翻到最后一部分）：https://www.cnblogs.com/aishangJava/p/5971288.html

spring-boot官方文档（英文）：https://docs.spring.io/spring-boot/docs/current/reference/pdf/spring-boot-reference.pdf



### QuickHttp实践

QuickHttp是一个超级简单易用的Http访问框架，用于快速生成Http请求并执行访问。

入手教程、GitHub库地址：https://github.com/fcibook/QuickHttp

Maven依赖：

```xml
<dependency>
    <groupId>com.fcibook.quick</groupId>
    <artifactId>quick-http</artifactId>
    <version>1.3</version>
</dependency>
```

**【注意】另外还需要安装的依赖：**

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>fluent-hc</artifactId>
    <version>4.4.1</version>
</dependency>
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpmime</artifactId>
    <version>4.4.1</version>
</dependency>
```



## 规范

### 工具版本

#### java版本

请使用oracle官网提供的jdk8版本（笔者使用的版本是`jdk-8u171-windows-x64`）

### 代码规范



### 设计规范

#### 单例模式的设计

单例模式的常见写法见此博客：https://www.cnblogs.com/zhaoyan001/p/6365064.html

推荐使用**双重检查**和**静态内部类**写法，以保证性能和线程安全性。（笔者本人推荐使用静态内部类写法）

* 双重检查示例

  ```java
  public class Singleton {
  
      private static volatile Singleton singleton;
  
      private Singleton() {}
  
      public static Singleton getInstance() {
          if (singleton == null) {
              synchronized (Singleton.class) {
                  if (singleton == null) {
                      singleton = new Singleton();
                  }
              }
          }
          return singleton;
      }
  }
  ```



* 静态内部类示例

  ```java
  public class Singleton {
  
      private Singleton() {}
  
      private static class SingletonInstance {
          private static final Singleton INSTANCE = new Singleton();
      }
  
      public static Singleton getInstance() {
          return SingletonInstance.INSTANCE;
      }
  }
  ```

  

以上





