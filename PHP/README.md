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

### 类型转换的判别
PHP 在变量定义中不需要(或不支持)明确的类型定义;变量类型是根据使用该变量的上下文所决定的.也就是说，如果把一个 string 值赋给变量 $var ,$var 就成了一个 string .如果又把 integer 赋给 $var ,那它就成了一个 integer .
```PHP
<?php
$foo = "1"; // $foo 是字符串(ASCII 49)
$foo *= 2; // $foo 现在是一个整数(2)
$foo = $foo *1.3; // $foo 现在是一个浮点数(2.6)
$foo = 5 * "10 Little Piggies"; // $foo 是整数(50)
$foo = 5 * "10 Small Pigs"; // $foo 是整数(50)
?>
```
如果上面两个例子看上去古怪的话，参见**字符串转换为数值**.

* [字符串转换为数值](http://php.net/manual/zh/language.types.string.php#language.types.string.conversion)

如果要强制将一个变量当做某种类型来求值,参见**类型强制转换**一节.如果要改变一个变量的类型,参见**settype()**.

* [类型强制转换](http://php.net/manual/zh/language.types.type-juggling.php#language.types.typecasting)
* [settpye()](http://php.net/manual/zh/function.settype.php)

如果想要测试本节中任何例子的话，可以用 var_dump() 函数.

**Note**:自动转换为数组的行为目前没有定义.
此外,由于 PHP 支持使用和数组下标同样的语法访问字符串下标,一下例子在所有 PHP 版本中都有效:
```PHP
<?php
$a = 'car'; // $a is a string
$a[0] = 'b'; // $a is still a string
echo $a; // bar
?>
```
#### 类型强制转换
PHP 中的类型强制转换和 C 中的非常像:在要转换的变量之前加上用括号括起来的目标类型.
```PHP
<?php
$foo = 10; // $foo is an integer
$bar = (boolean) $foo; // $bar is a boolean
?>
```
允许的强制转换有:

* (int),(integer) - 转换为整形 integer
* (bool),(boolean) - 转换为布尔类型 boolean
* (float),(double),(real) - 转换为浮点型 float
* (string) - 转换为字符串 string
* (array) - 转换为数组 array
* (object) - 转换为对象 object
* (unset) - 转换为 NULL(PHP5)

(binary)转换和 b 前缀转换支持为 PHP 5.2.1 新增.
注意在括号内允许有空格和制表符,所以下面两个例子功能相同:
```PHP
<?php
$foo = (int) $bar;
$foo = ( int ) $bar;
?>
```
将字符串文字和变量转换为二进制字符串:
```PHP
<?php
$binary = (binary)$string;
$binary = b"binary string";
?>
```
**Note**:可以将变量放置在双引号中的方式来代替将变量转换成字符串:
```PHP
<?php
$foo = 10; // $foo 是一个整数
$str = "$foo"; // $str 是一个字符串
$fst = (string) $foo; // $fst 也是一个字符串
// 输出 "they are the same"
if ($fst === $str){
    echo "they are the same";
}
?>
```

## 变量
### 变量范围

变量的范围即它定义的上下文背景(也就是它的生效范围).大部分的 PHP 变量只有一个单独的范围.这个单独的范围跨度同样包含了 include 和 require 引入的文件.例如:

```PHP
<?php
$a = 1;
include 'b.inc';
?>
```
这里的变量 $a 将会在包含文件 b.inc 中生效.但是,在用户自定义函数中,一个局部函数范围将被引入.任何用于函数内部的变量按缺省情况将被限制在局部函数范围内.例如:

```PHP
<?php
$a = 1; /* global scope */

function Test()
{
    echo $a; /* reference to local scope variable */
}

Test();
?>
```

![variable.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/variable1.png)

这个脚本不会有任何输出,因为 echo 语句引用了一个局部版本的变量 $a ,而且在这个范围内,它并没有被赋值.你可能注意到 PHP 的全局变量和 C 语言有一点点不同,在 C 语言中,全局变量在函数中自动生效,除非被局部变量覆盖.这可能引起一些问题,有些人可能不小心就改变了一个全局变量.PHP 中全局变量在函数中使用时必须声明为 global .

#### global 关键字
首先,一个使用 global 的例子:

Example #1 使用 global 
```PHP
<?php
$a = 1;
$b = 2;
function Sum()
{
    global $a,$b;
    $b = $a + $b;
}
Sum();
echo $b;
?>
```
以上的脚本输出将是3.在函数中声明了全局变量 $a 和 $b 后,变量就可以获取到函数外的值.对于一个函数能够声明的全局变量的最大个数,PHP 没有限制.

在全局范围内访问变量的第二个方法,是用特殊的 PHP 自定义 $GLOBALS 数组.前面的例子可以写成:

Example #2 使用 $GLOBALS 替代 global
```PHP
<?php
$a = 1;
$b = 2;
function Sum()
{
    $GLOBALS['b'] = $GLOBALS['a'] + $GLOBALS['b'];
}
Sum();
echo $b;
?>
```
$GLOBALS 是一个关联数组,每一个变量为一个元素,键名对应变量名,值对应变量的内容.$GLOBALS 之所以在全局范围内存在,是因为 $GLOBALS 是一个超全局变量.

以下范例显示了超全局变量的用处:

Example #3 演示超全局变量和作用域的例子
```PHP
<?php
function test_global()
{
    // 大多数的预定义变量并不 "super",它们需要用 'global' 关键字来使它们在函数的本地区域中有效.
    global $HTTP_POST_VARS;

    echo $HTTP_POST_VARS['name'];

    // Superglobals 在任何范围内都有效,它们并不需要 'global' 声明.Superglobals 是在 PHP 4.1.0 引入的.
    echo $_POST['name'];
}
?>
```

#### 使用静态变量
变量范围的另一个重要特性是*静态变量*(static varialbe).静态变量仅在函数域中存在,但当程序执行离开此作用域时,其值并不丢失.

Example #4 演示需要静态变量的例子
```PHP
<?php
function Test()
{
    $a = 0;
    echo $a;
    $a++;
}
?>
```
以上的例子在每次调用时都会将 $a 的值设为 0 并输出 0.函数中的 $a++ 没有作用，因为一旦退出本函数则变量 $a 就不存在了.要写一个不会丢失本次计数值的计数函数,要将变量 $a 定义为静态的:

Example #5 使用静态变量的例子
```PHP
<?php
function test()
{
    static $a = 0;
    echo $a;
    $a++;
}
?>
```
现在,变量 $a 仅在第一次调用 test() 函数时被初始化,之后每次调用 test() 函数都会输出 $a 的值并加一.
静态变量也提供了一种处理递归函数的方法.递归函数时一种调用自己的函数.写递归函数时要小心,因为可能会无穷递归下去.必须确保有充分来中止递归.以下这个简单的函数递归计数到 10 ,使用静态变量 $count 来判断何时停止:
Example #6 静态变量与递归函数
```PHP
<?php
function test()
{
    static $count = 0;
    $count++;
    echo $count;
    if($count<10){
        test();
    }
    $count--;
}
?>
```

**Note**:静态变量可以按照上面的例子声明.如果在声明中用表达式的结果对其赋值会导致解析错误.
Example #7 声明静态变量
```PHP
<?php
function foo(){
    static $int = 0; // correct
    static $int = 1+2; // wrong (as it is an expression)
    static $int = sqrt(121); //wrong (as it is an expression too)
    $int++;
    echo $int;
}
?>
```
静态声明是在编译时解析的.

**Note**:在函数之外使用 global 关键字不算错.可以用于在一个函数之内包含文件时.

#### 全局和静态变量的引用
在 Zend 引擎 1 代,它驱动了 PHP4 ,对于变量的 static 和 global 定义是以引用的方式实现的.例如,在一个函数域内部用 global 语句导入的一个真正的全局变量实际上是建立了一个到去全局变量的引用.这有可能导致预料之外的行为,如以下例子所演示的:
```PHP
<?php
function test_global_ref(){
    global $obj;
    $obj = &new stdclass;
}
function test_global_noref(){
    global $obj;
    $obj = new stdclass;
}
test_global_ref();
var_dump($obj);
test_global_noref();
var_dump($obj);
?>
```
以上例程会输出:

![global1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/global1.png)

类似的行为也适用于 static 语句.引用并不是静态地存储的:
```PHP
<?php
function &get_instance_ref(){
    static $obj;
    echo 'Static object:';
    var_dump($obj);
    if(!isset($obj)){
        // 将一个引用赋值给静态变量
        $obj = &new stdclass;
    }
    $obj->property++;
    return $obj;
}
function &get_instance_noref(){
    static $obj;
    echo 'Static object:';
    var_dump($obj);
    if(!isset($obj)){
        // 将一个对象赋值给静态变量
        $obj = new stdclass;
    }
    $obj->property++;
    return $obj;
}
$obj1 = get_instance_ref();
$still_obj1 = get_instance_ref();
echo "\n";
$obj2 = get_instance_noref();
$still_obj2 = get_instance_noref();
?>
```
以上例程会输出:
![global2.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/global2.png)
上例演示了当把一个引用赋值给一个静态变量时,第二次调用 &get_instance_ref() 函数时其值并没有被*记住*.

### 可变变量
有时候使用可变变量名是很方便的.就是说,一个变量的变量名可以动态的设置和使用.一个普通的变量通过声明来设置,例如:
```PHP
<?php
$a = 'hello';
?>
```
一个可变变量获取了一个普通变量的值作为这个可变变量的变量名.在上面的例子中 hello 使用了两个美元符号($)以后,就可以作为一个可变变量的变量了.例如:
```PHP
<?php
$$a = 'world';
?>
```
这时,两个变量都被定义了:$a 的内容是"hello"并且 $hello 的内容是 "world" .因此,以下语句:
```PHP
<?php
echo "$a ${$a}";
?>
```
与以下语句输出完全相同的结果:
```PHP
<?php
echo "$a $hello";
?>
```

它们都会输出: hello world.

**Note**:可变变量用于数组会出现一个模棱两可的问题.就是当写下 $$a[1] 时,解析器需要知道是想要 $a[1] 作为一个变量,还是想要 $$a 作为一个变量并取出该变量中索引并取出该变量中索引为[1]的值.解决此问题的语法是,对第一个情况用 ${$a[1]},对第二种情况用 ${$a}[1].

类的属性也可以通过可变属性名来访问.可变属性名将在该调用所处的范围内被解析.例如,对于 $foo->$bar 表达式,则会在本地范围来解析 $bar 并且其值将被用于 $foo 的属性名.对于 $bar 是数组单元时也是一样.
也可使用花括号来给属性名清晰定界.最有用是在属性位于数组中,或者属性名包含有多个部分或者属性名包含有非法字符时(例如来自 json_decode() 或 SimpleXML).

Example #1 可变属性示例
```PHP
<?php
class foo{
    var $bar = 'I am bar.';
    var $arr = array('I am A.','I am B.','I am C.');
    var $r = 'I am r.';
}
$foo = new foo();
$bar = 'bar';
$baz = array('foo','bar','baz','quux');
echo $foo->$bar."\n";
echo $foo->$baz[1]."\n";

$start = 'b';
$end = 'ar';
echo $foo->{$start . $end}."\n";

$arr = 'arr';
echo $foo->$arr[1]."\n";
echo $foo->{$arr}[1]."\n";
?>
```
以上例程会输出:
![variables1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/variables1.png)

**Warning** 注意,在 PHP 的函数和类的方法中,超全局变量不能用作可变变量.$this 变量也是一个特殊变量,不能被动态引用.

### 来自 PHP 之外的变量
#### HTML 表单(GET 和 POST)
当一个表单提交给 PHP 脚本时,表单中的信息会自动在脚本中可用.有很多方法访问此信息,例如:
Example #1 一个简单的 HTML 表单
```HTML
<form action="foo.php" method="post">
    Name: <input type="text" name="username"><br />
    Email: <input type="text" name="eamil"><br />
    <input type="submit" name="submit" value="Submit me!" />
</form>
```
根据特定的设置和个人的喜好,有很多种方法访问 HTML 表单中的数据.例如:
Example #2 从一个简单的 POST HTML 表单访问数据
```PHP
<?php
// 自 PHP 4.1.0 起可用
    echo $_POST['username'];
    echo $_REQUEST['username'];
    
    import_request_variables('p','p_');
    echo $p_username;

// 自 PHP 5.0.0 起,这些长格式的预定义变量
// 可用 register_long_arrays 指令关闭.

    echo $HTTP_POST_VARS['username'];

// 如果 PHP 指令 register_globals = on 时可用.不过自
// PHP 4.2.0 起默认值为 register_globals = off.
// 不提倡使用/依赖此种方法.

    echo $username;
?>
```
使用 GET 表单也类似,只不过要用适当的 GET 预定义变量.GET 也适用于 QUERY_STRING(URL 中在"?"之后的信息).因此,举例说,http://www.example.com/test.php?id=3 包含有可用 $_GET['id'] 来访问的 GET 数据.

**Note**:超全局数组例如 $_POST 和 $_GET ,自 PHP 4.1.0 起可用.

**Note**:变量名中的点和空格被转换成下划线.例如<input name="a.b" /> 变成了 $_REQUEST["a_b"].

如上所示,在 PHP 4.2.0 之前 register_globals 的默认值是 on.PHP 社区鼓励大家不要依赖此指令,建议在编码时假定其为 off.

**Note**:magic_quotes_gpc 配置指令影响到 Get,Post 和 Cookie 的值.如果打开,值(It's "PHP!")会自动转换成(It\'s \"PHP!\").十多年前对数据库的插入需要如此转义,如今已经过时了,应该关闭.

PHP 也懂得表单变量上下文中的数组.例如可以将相关的变量编成组,或者用此特性从多选输入框中取得值.例如,将一个表单 POST 给自己并在提交时显示数据:

Example #3 更复杂的表单变量
```PHP
<?php
if(isset($_POST['action'])&&$_POST['action'] == 'submitted')
{
    echo '<pre>';
    print_r($_POST);
    echo '<a href="'.$_SERVER['PHP_SELF'].'">Please try again</a>'>;
    echo '</pre>';
}else{
?>
<form action="<?php echo $_SERVER['PHP_SELF'];?>" method="post">
    Name:<input type="text" name="personal[name]"><br />
    Email:<input type="text" name="personal[eamil]"><br />
    Beer:<br>
    <select multiple name="beer[]">
        <option value="warthog">Warthog</option>
        <option value="guinness">Guinness</option>
        <option value="stuttgarter">Stuttgarter Schwabenbr</option>
    </select><br />
    <input type="hidden" name="action" value="submitted" />
    <input type="submit" name="submit" value="submit me!" />
</form>
<?php
}
?>
```
#### IMAGE SUBMIT 变量名
当提交表单时,可以用一幅图像代替标准的提交按钮,用类似这样的标记:
```HTML
<input type="image" src="image.gif" name="sub" />
```
当用户点击到图像中的某处时,相应的表单会被传送到服务器,并加上两个变量 sub_x 和 sub_y.它们包含了用户点击图像的坐标.有经验的用户可能会注意到被浏览器发送的实际变量名包含的是一个点而不是下划线(即 sub.x 和 sub.y),但 PHP 自动将点转换成了下划线.

#### HTTP Cookies
PHP 透明地支持 >> RFC 6265 定义中的 HTTP cookies.Cookies 是一种在远端浏览器端存储数据并能追踪或识别再次访问的用户的机制.可以用 setcookie() 函数设定 cookies.Cookies 是 HTTP 信息头中的一部分,因此 SetCookie 函数必须在向浏览器发送任何输出之前调用.对于 header() 函数也有同样的限制.Cookie 数据会在相应的 cookie 数据数组中可用,例如 $_COOKIE,$HTTP_COOKIE_VARS 和 $_REQUEST.更多细节和例子见 setcookie() 手册页面.

如果要将多个值赋给一个 cookie 变量,必须将其赋成数组.例如:
```PHP
<?php
    setcookie("MyCookie[foo]",'Testing 1',time()+3600);
    setcookie("MyCookie[bar]",'Testing 2',time()+3600);
?>
```

这将会建立两个单独的 cookie,尽管 MyCookie 在脚本中是一个单一的数组.如果想在仅仅一个 cookie 中设定多个值,考虑先在值上使用 serialize() 或 explode().
注意在浏览器中一个 cookie 会替换掉上一个同名的 cookie,除非路径或者域不同.因此对于购物车程序可以保留一个计数器并一起传递,例如:
Example #4 一个 setcookie() 的示例

```PHP
<?php
if(isset($_COOKIE['count'])){
    $count = $_COOKIE['count'] + 1;
} else {
    $count = 1;
}
setcookie('count',$count,time()+3600);
sercookie("Cart[$count]",$item,time()+3600);
?>
```

#### 变量名中的点
通常,PHP 不会改变传递给脚本中的变量名.然而应该注意到点(句号)不是 PHP 变量名中的合法字符.至于原因,看看:
```PHP
<?php
$varname.ext; /* 非法变量名 */
?>
```
这时,解析器看到是一个名为 $varname 的变量,后面跟着一个字符串连接运算符,后面跟着一个裸字符串(即没有加引号的字符串,且不匹配任何已知的键名或保留字)'ext'.很明显这不是想要的结果.
出于此原因,要注意 PHP 将会自动将变量名中的点替换成下划线.

#### 确定变量类型
因为 PHP 会判断变量类型并在需要时进行转换(通常情况下),因此在某一时刻给定的变量是何种类型并不明显.PHP 包括几个函数可以判断变量的类型,例如:gettype(),is_array(),is_float(),is_int(),is_object() 和 is_string().
## 流程控制
### 流程控制的替代语法

PHP 提供了一些流程控制的替代语法，包括 if,while,for,foreach 和 switch。替代语法的基本形式是把左花括号({)换成冒号(:)，把右花括号(})分别换成 endif;,endwhile;,endfor;,endforeach; 以及 endswitch;。
例：

```PHP
<? php if($a == 5): ?>
A is equal to 5
<? php endif; ?>
```

在 else 和 elseif 中：

```PHP
<?php
if($a == 5):
    echo "a equals 5";
    echo "...";
elseif($a == 6):
    echo "a equals 6";
    echo "!!!";
else:
    echo "a is neither 5 nor 6";
endif;
?>
```

**Note**:不支持在同一个控制块内混合使用两种语法。

**Warning** switch 和第一个 case 之间的任何输出(含空格)将导致语法错误。例如，这样是无效的：

```PHP
<?php switch($foo): ?>
    <?php case 1: ?>
    ...
<?php endswitch ?>
```

而这样是有效的，因为 switch 之后的换行符被认为是结束标记 ?> 的一部分，所以在 switch 和 case 之间不能有任何输出：

```PHP
<?php switch($foo): ?>
<?php case 1: ?>
...
<?php endswitch ?>
```
### foreach
foreach 语法结构提供了遍历数组的简单方式。foreach 仅能够应用于数组和对象，如果尝试应用于其他数据类型的变量，或者未初始化的变量将发出错误信息。有两种语法：
```PHP
foreach (array_expression as $value)
    statement
foreach (array_expression as $key => $value)
    statement
```
第一种格式遍历给定的 array_expression 数组。每次循环中，当前单元的值被赋给 $value 并且数组内部的指针向前移一步(因此下一次循环中将会得到下一个单元)。
第二种格式做同样的事，只除了当前单元的键名也会在每次循环中被赋给变量 $key。
**Note:**
当 foreach 开始执行时，数组内部的指针会自动指向第一个单元。这意味着不需要在 foreach 循环之前调用 reset()。

由于 foreach 依赖内部数组指针，在循环中修改其值将可能导致意外的行为。

可以很容易地通过在 $value 之前加上 & 来修改数组的元素。此方法将以引用赋值而不是拷贝一个值。
```PHP
<?php
$arr = array(1,2,3,4);
foreach ($arr as &$value){
    $value = $value * 2;
}
// $arr is now array(2,4,6,8)
unset($value);// 最后取消掉引用
?>
```
$value 的引用仅在被遍历的数组可以被引用时才可用(例如是个变量)。以下代码则无法运行：
```PHP
<?php
foreach (array(1,2,3,4) as &$value){
    $value = $value *2;
}
?>
```
**Warning** 数组最后一个元素的 $value 引用在 foreach 循环之后仍会保留。建议使用 unset() 来将其销毁。

**Note**:
foreach 不支持用"@"来抑制错误信息的能力。

#### 用 list() 给嵌套的数组解包
PHP 5.5 增添了遍历一个数组的数组的功能并且把嵌套的数组解包到循环变量中，只需将 list() 作为值提供。
例如:
```PHP
<?php
$array = [
    [1,2],
    [3,4],
];
foreach ($array as list($a,$b)){
    // $a contains the first element of the nested array,
    // and $b contains the second element.
    echo "A: $a; B:$b\n";
}
?>
```
输出结果：

![PHP08](https://github.com/alreadyaabb/blog/blob/master/images/PHP08.png)

list() 中的单元可以少于嵌套数组的，此时多出来的数组单元将被忽略：
```PHP
<?php
$array = [
    [1,2],
    [3,4],
];
foreach ($array as list($a)){
    // Note that there is no $b here.
    echo "$a\n";
}
?>
```
输出结果：

![PHP09](https://github.com/alreadyaabb/blog/blob/master/images/PHP09.png)

如果 list() 中列出的单元多于嵌套数组则会发出一条消息级别的错误信息:
```PHP
<?php
$array = [
    [1,2],
    [3,4],
];
foreach ($array as list($a,$b,$c)){
    echo "A: $a; B: $b; C: $c\n";
}
?>
```
输出结果:

![PHP10](https://github.com/alreadyaabb/blog/blob/master/images/PHP10.png)

### break
break 结束当前 for,foreach,while,do-while 或者 switch 结构的执行。
break 可以接受一个可选的数字参数来决定跳出几重循环。
```PHP
<?php
$arr = array('one','two','three','four','stop','five');
while (list(, $val) = each($arr)){
    if ($val == 'stop'){
        break; /* You could also write 'break 1;' here. */
    }
    echo "$val<br>\n";
}
/* 使用可选参数 */
$i = 0;
while (++$i){
    switch ($i){
        case 5:
            echo "At 5<br>\n";
            break 1; /*只退出 switch. */
        case 10:
            echo "At 10; quitting<br>\n";
            break 2; /*退出 switch 和 while 循环 */
        default:
            break;
    }
}
?>
```

### continue

continue 在循环结构中用来跳过本次循环中剩余的代码并在条件求值为真时开始执行下一次循环。

**Note:**注意在 PHP 中 switch 语句被认为是可以使用 continue 的一种循环结构。

continue 接受一个可选的数字参数来决定跳过几重循环到循环结尾。默认值是1，即跳到当前循环末尾。
省略 continue 后面的分号会导致混淆。以下例子示意了不应该这样做。
```PHP
<?php
for ($i = 0;$i < 5; ++ $i){
    if ($i == 2)
        continue
    print "$i\n";
}
?>
```
希望得到的结果是：

![PHP11](https://github.com/alreadyaabb/blog/blob/master/images/PHP11.png)

可实际的输出是：

![PHP12](https://github.com/alreadyaabb/blog/blob/master/images/PHP12.png)

因为整个 continue print "$i\n"; 被当作单一的表达式而求值，所以 print 函数只有在 $i ==2 为真时才被调用(print 的值被当成了上述的可选数字参数而传递给了 continue)。

### switch
switch 语句类似于具有同一个表达式的一系列 if 语句。很多场合下需要把同一个变量(或表达式)与很多不同的值比较，并根据它等于哪些值来执行不同的代码。这正是 switch 语句的用途。

**Note**:注意和其它语言不同，continue 语句作用到 switch 上的作用类似于 break。如果在循环中有一个 switch 并希望 continue 到外层循环中的下一轮循环，用 continue 2。

**Note**:注意 switch/case 作的是松散比较。

下面两个例子使用两种不同方法实现同样的事，一个用一系列的 if 和 elseif 语句，另一个用 switch 语句：

**Example #1** switch 结构

```PHP
<?php
if ($i == 0){
    echo "i equals 0";
} elseif ($i == 1){
    echo "i equals 1";
} elseif ($i == 2){
    echo "i equals 2";
}
switch ($i){
    case 0:
        echo "i equals 0";
        break;
    case 1:
        echo "i equals 1";
        break;
    case 2:
        echo "i equals 2";
        break;
}
?>
```

**Example #2** switch 结构可以用字符串

```PHP
<?php
switch ($i){
    case "apple":
        echo "i is apple";
        break;
    case "bar":
        echo "i is bar";
        break;
    case "cake":
        echo "i is cake";
        break;
}
?>
```

**Note**:每条 case 语句段最后都要写上 break 。否则 PHP 将继续执行下一个 case 中的语句段。例如：

```PHP
<?php
switch ($i){
    case 0:
        echo "i equals 0";
    case 1:
        echo "i equals 1";
    case 2:
        echo "i equals 2";
}
?>
```
这里如果 $i 等于 0，PHP 将执行所有的 echo 语句！如果 $i 等于 1，PHP 将执行后面两条 echo 语句。只有当 $i 等于 2 时，才会得到"预期"的结果——只显示"i equals 2"。

利用 switch 语句的这一特点，我们可以简化代码书写。
```PHP
<?php
switch ($i){
    case 0:
    case 1:
    case 2:
        echo "i is less than 3 but not negative";
        break;
    case 3:
        echo "i is 3";
}
?>
```
一个 case 的特例是 default。它匹配了任何和其它 case 都不匹配的情况。例如：
```PHP
<?php
switch ($i){
    case 0:
        echo "i equals 0";
        break;
    case 1:
        echo "i equals 1";
        break;
    case 2:
        echo "i equals 2";
        break;
    default:
        echo "i is not equal to 0, 1 or 2";
}
?>
```
case 后的冒号可以用分号代替：
```PHP
<?php
switch ($beer)
{
    case 'tuborg';
    case 'carlsberg';
    case 'heineken';
        echo 'Good choice';
        break;
    default;
        echo 'Please make a new selection...';
        break;
}
?>
```
References:

* [Official Website](http://php.net)
* [PSR](http://www.php-fig.org/)
* [PHP100](http://www.php100.com/)

