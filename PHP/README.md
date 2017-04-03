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

![NetBeans SFTP配置]()


### 用 XDebug 实现远程调试



References:

* [Official Website](http://php.net)
* [PSR](http://www.php-fig.org/)
* [PHP100](http://www.php100.com/)

