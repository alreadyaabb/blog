# PHP

此文档介绍了如何使用阿里云搭建 PHP 服务，并在 Windows 环境下搭建高效开发和调试环境。

## 依赖的软件和系统环境：

* [Ubuntu Server 16.04.2 LTS](https://www.ubuntu.com/download/server)
* [PHP 7.0](http://php.net/)
* [NetBeans 8.2](https://netbeans.org/downloads/)
* [XDebug 2.4.0](https://xdebug.org/)
* [XShell 5](https://www.netsarang.com/xshell_download.html)
* [MobaXterm 9.3](http://mobaxterm.mobatek.net/)

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

到 Oracle 官网下载最新的 JDK，并到 NetBeans官网下载最新版本的 PHP 开发环境安装即可。
由于笔者并不熟悉Java环境的配置，这里请自行 Google .

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
SFTP配置](https://github.com/alreadyaabb/blog/blob/master/images/netbeans-01.png)


### 用 XDebug 实现远程调试



References:

* [Official Website](http://php.net)
* [PSR](http://www.php-fig.org/)
* [PHP100](http://www.php100.com/)

