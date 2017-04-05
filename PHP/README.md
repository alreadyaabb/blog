# PHP

此文档介绍了如何使用阿里云搭建 PHP 服务，并在 Windows 环境下搭建高效开发和调试环境。

## 依赖的软件和系统环境：

* [Ubuntu Server 16.04.2 LTS](https://www.ubuntu.com/download/server)
* [PHP 7.0](http://php.net/)
* [NetBeans 8.2](https://netbeans.org/downloads/)
* [XDebug 2.4.0](https://xdebug.org/)
* [XShell 5](https://www.netsarang.com/xshell_download.html)
* [MobaXterm 9.3](http://mobaxterm.mobatek.net/)
* [Oracle JDK-8u121](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

## PHP 开发环境搭建

Ubuntu Server 16.04.2 TLS 是阿里云自带的服务器操作系统，可以通过 `apt` 命令安装最新版本的 PHP 
，笔者在记录此文档时，使用的是 7.0 版本，以下配置应该也适用于其他版本。

首先安装好上一个章节依赖的软件。

### PHP 服务端环境安装

```shell
# 通过 apt 命令安装 PHP 和 需要的扩展
apt install apache2 apache2-utils php php-xdebug
```

### NetBeans 安装和配置

> 因为笔者使用的是 Windows 操作系统，所以这里仅介绍 Windows 下 NetBeans 的安装。

NetBeans 是基于 Java 开发的集成开发环境，所以想要 NetBeans 能正常运行，需要首先安装 JDK ，
先到 Oracle 的官网下载最新版本的 JDK ，如下图：

![JDK Download](https://github.com/alreadyaabb/blog/blob/master/images/jdk-download.png)

根据提示直接安装即可，不需要做 *任何配置* 。

找到 NetBeans 的官网，下载最新版本的 NetBeans For PHP 版本，如下图：

![NetBeans Download](https://github.com/alreadyaabb/blog/blob/master/images/netbeans-download.png)

根据提示安装即可。

#### 远程项目开发和部署

第一步进行 NetBeans 上对于项目的设置：

1. 创建一个 PHP Application,在 Run Configuration 步骤下，将 Run As 选项选为 Remote Web Site 
，点击 Manage... 按钮打开窗口，在 Connection Name 中给连接命名，在 Connection Type 中选择 SFTP 选项，单击 
OK 键打开 Manage Remote Connections 窗口。
2. 在 Host Name 中填写服务器的 IP 地址，在 User Name 和 Password 中分别填写服务器的用户名与密码，然后单击 OK 
键，保存配置。将 Upload Files 选为 On Save，点击 Finish 创建项目。
3. 右键单击 Project 打开 Properties ，选择 Run Configuration 在右侧的 Project URL 中将 
localhost 改为运行程序时访问的URL地址，在 Upload Directory 中填写相对于服务器源文件目录的相对目录，然后单击 OK 键。

第二步进行代码的远程上传与同步：

1. 右键单击 Source Files 文件夹选择 synchronize 进行文件同步。
2. 当完成代码时，右键单击 Source Files 文件夹选择 Upload 进行文件上传。

![NetBeans 
SFTP配置1](https://github.com/alreadyaabb/blog/blob/master/images/netbeans-01.png)

![NetBeans 
SFTP配置2](https://github.com/alreadyaabb/blog/blob/master/images/netbeans-02.png)

### 用 XDebug 实现远程调试

配置 XDebug 的过程一共分为三步，分别是在服务器， NetBeans 和 XShell 中配置。

第一步在服务器上配置 XDebug ：

打开 xdebug.ini 文件添加如下配置

```shell
vi /etc/php/7.0/mod-available/xdebug.ini
```

```ini
xdebug.show_error_trace=1
xdebug.remote_enable=On
xdebug.remote_host=127.0.0.1
xdebug.remote_port=9000
xdebug.remote_handler=dbgp
```
**切记**:配置过服务器的文件后，一定要重启 Apache 服务器。

第二步在 NetBeans 中配置 XDebug：

![NetBeans 
XDebug配置](https://github.com/alreadyaabb/blog/blob/master/images/netbeans-03.png
)
1. 点击 Tools 选择 Options 将 Debugger Port 更改为与服务器中的端口号一致
2. 将 Watches and Balloon Evaluation , Show Requested URLs , Show Debugger 
Console 勾选

第三步在 XShell 中配置 XDebug：

1. 点击 File 选择 New 新建一个 Session ，在 Name 中为 Session 命名，在 Host 中填写主机 IP 地址。
2. 点击窗口左侧的 Tunneling 然后点击右侧的 Add... 按钮打开 Forwarding Rule 窗口。
3. 将 Type 改为 Remote ，在 Source Host 中填写服务器的 IP 地址，在 Listening Port 
中填写与服务器端口一致的端口号，在 Destination Host 中选择 localhost ，在 Destination Port 中填写与 
NetBeans 端口号一致的端口号。

**注意** 一般会出现的问题有：
1. 更改完服务器配置忘记重启 Apache 服务器。
解决方法：
```shell
sudo /etc/init.d/apache2 restart
```
2. 9000 端口被占用。
解决方法：
```shell
kill -9 $(lsof -t -i tcp:9000)
```
使用此条命令可以将占用 9000 的端口强制关闭。

References:

* [Official Website](http://php.net)
* [PSR](http://www.php-fig.org/)
* [PHP100](http://www.php100.com/)

