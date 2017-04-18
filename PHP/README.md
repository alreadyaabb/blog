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
使用此条命令可以将占用 9000 的端口强制关闭。
```

### 基本语法
#### PHP标记
当解析一个文件时，PHP 会寻找起始和结束标记，也就是 <?php 和 ?> ,这告诉 PHP 开始和停止解析二者之间的代码。此种解析方式使得 PHP 可以被嵌入到各种不同的文档中去，而任何起始和结束标记之外的部分都会被 PHP 解析器忽略。
PHP 也允许使用短标记 <? 和 ?> ,但不鼓励使用。只有通过激活 php.ini 中的 short_open_tag 配置指令或者在编译 PHP 时使用了配置选项 --enable-short-tags 时才能使用短标记。
如果文件内容是纯 PHP 代码，最好在文件末尾删除 PHP 结束标记。这可以避免在 PHP 结束标记之后万一意外加入了空格或者换行符，会导致 PHP 开始输出这些空白，而脚本中此时并无输出的意图。
```php
<?php
echo "Hello world";
// ... more code
echo "Last statement";
// 脚本至此结束，并无结束标记
```

#### 从 HTML 中分离
凡是在一对开始和结束标记之外的内容都会被 PHP 解析器忽略，这使得 PHP 文件可以具备混合内容。可以使 PHP 嵌入到 HTML 文档中去，如下例所示：
```php
<p>This is going to be ignored by PHP and displayed by the browser.</p>
<?php echo 'While this is going to be parsed.'; ?>
<p>This will also be ignored by PHP and displayed by the browser.</p>
```
这将如预期中的运行，因为当 PHP 解释器碰到 ?> 结束标记时就简单地将其后内容原样输出(除非马上紧接换行-见指令分隔符)直到碰到下一个开始标记；例如是处于条件语句中间时，此时 PHP 解释器会根据条件判断来决定哪些输出，哪些跳过。见下例。

使用条件结构：

Example #1 使用条件的高级分离术
```php
<?php if($expression == true); ?>
    This will show if the expression is true.
<?php else: ?>
    Otherwise this will show.
<?php endif; ?>
```

上例中 PHP 将跳过条件语句未打成的段落，即使该段落位于 PHP 开始和结束标记之外。由于 PHP 解释器会在条件未达成时直接跳过该条件语句块，因此 PHP 会根据条件来忽略之。

要输出大段文本时，跳出 PHP 解析模式通常比将文本通过 echo 或 print 输出更有效率。

可以在 PHP 中使用四对不同的开始和结束标记。其中两种，<?php ?> 和 <script language="php"></script>总是可用的。另两种是短标记和 ASP 风格标记很方便，但移植性较差，通常不推荐使用。

**Note:**
此外注意如果将 PHP 嵌入到 XML 或 XHTML 中则需要使用 <?php ?> 标记以保持符合标准。

Example #2 PHP 开始和结束标记

![php1.png](https://github.com/alreadyaabb/blog/blob/master/images/PHP01.png)

上例中的1和2中使用的标记总是可用的，其中示例1中是最常用的，并建议使用的。短标记(上例3)仅在通过 php.ini 配置文件中的指令 short_open_tag 打开后才可用，或者在 PHP 编译时加入了 --enable-short-tags 选项。

ASP 风格标记(上例4)仅在通过 php.ini 配置文件中的指令 asp_tags 打开后才可使用。

**Note:**
在以下情况应避免使用短标记：开发需要再次发布的程序或者库，或者在用户不能控制的服务器上开发。因为目标服务器可能不支持短标记。为了代码的移植及发行，确保不要使用短标记。

**Note:**
在 PHP5.2 和之前的版本中，解释器不允许一个文件的全部内容就是一个开始标记 <?php .自 PHP5.3 起则允许此种文件，但要开始标记后有一个或更多空白格符。

**Note:**
自 PHP5.4 起，短格式的 echo 标记<?= 总会被识别并且合法，而不管short_open_tag 的设置是什么。

#### 指令分隔符
同 C 或 Perl 一样，PHP 需要在每个语句后用分号结束指令。一段 PHP 代码中的结束标记隐含表示了一个分号；在一个 PHP 代码段中的最后一行可以不用分号结束。如果后面还有新行，则代码段的结束标记包含了行结束。
```php
<?php
    echo "This is a test";
?>
<?php echo "This is a test" ?>
<?php echo 'We omitted the last closing tag';
```

**Note:**
文件末尾的 PHP 代码段结束标记可以不要，有些情况下当使用 include 或者 require 时省略掉会更好些，这样不期望的空白符就不会出现在文件末尾，之后仍然可以输出响应标头。在使用输出缓冲时也很便利，就不会看到由包含文件生成的不期望的空白符

#### 注释
PHP 支持 C，C++ 和 Unix Shell 风格(Perl 风格)的注释。例如：
```php
<?php
    echo "This is a test";// This is a one-line c++ style comment
    /* This is a multi line comment
       yet another line of comment*/
    echo "This is yet another test";
    echo 'One Final Test';# This is a one-line shellstyle comment
?>
```
单行注释仅仅注释到行末或者当前的 PHP 代码块，视乎哪个首先出现。这意味着在 //...?> 或者 #...?>之后的 HTML 代码将被显示出来 :?> 跳出了 PHP 模式并返回了 HTML 模式，// 或 # 并不能影响到这一点。如果启用了 asp_tags 配置选项，其行为和 //%> 或 #%> 相同。不过，</script> 标记在单行注释中不会跳出 PHP 模式。
```php
<h1>This is an <?php # echo 'simple'; ?>
example</h1>
<p>The header above will say 'This is an example'.</p>
```

C 风格的注释在碰到第一个 */ 时结束。要确保不要嵌套 C 风格的注释。试图注释掉一大块代码时很容易出现该错误。

```php
<?php
/*
    echo "This is a test";/* This comment will cause a problem */
*/
?>
```

### 类型
#### 简介
PHP 支持 9 种原始数据类型。
四种标量类型：
* boolean(布尔型)
* integer(整型)
* float(浮点型，也称作double)
* string(字符串)
三种复合类型：
* array(数组)
* object(对象)
* callable(可调用)
最后是两种特殊类型：
* resource(资源)
* NULL(无类型)
为了确保代码的易读性，本文还介绍了一些伪类型：
* mixed(混合类型)
* number(数字类型)
* callback(回调类型，又称为callable)
* array|object(数组|对象类型)
* void(无类型)
以及伪变量$...。

可能还会读到一些关于"双精度(double)"类型的参考。实际上 double 和 float 是相同的，由于一些历史的原因，这两个名称同时存在。

变量的类型通常不是由程序员设定的，确切地说，是由 PHP 根据该变量使用的上下文在运行时决定的。

**Note:**
如果想查看某个表达式的值和类型，用 var_dump() 函数。
如果只是想得到一个易读懂的类型的表达方式用于调试，用 gettype() 函数。要检验某个类型，*不要*用 gettype(),而用 is_type() 函数。以下是一些范例：

![PHP02.png](https://github.com/alreadyaabb/blog/blob/master/images/PHP02.png)

如果要将一个变量强制转换为某类型，可以对其使用 **强制转换** 或者 **settype()** 函数。

注意变量根据其当时的类型在特定场合下会表现出不同的值。

References:

* [Official Website](http://php.net)
* [PSR](http://www.php-fig.org/)
* [PHP100](http://www.php100.com/)

