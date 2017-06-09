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
运行结果:

![variables2.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/variables2.png)

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

## 常量
常量是一个简单值的标识符(名字).如同其名称暗示的,在脚本执行期间该值不能改变(除了所谓的魔术常量,它们其实不是常量).常量默认为大小写敏感.传统上常量标识符总是大写的.
常量名和其他任何 PHP 标签遵循同样的命名规则.合法的常量名以字母或下划线开始,后面跟着任何字母,数字或下划线.用正则表达式是这样表达的:*[a-zA-Z\x7f-\xff][a-zA-Z0-9\x7f-xff]\**.
Example #1 合法与非法的常量名
```PHP
<?php

// 合法的常量名
define("FOO","something");
define("FOO2","something else");
define("FOO_BAR","something more");

// 非法的常量名
define("2FOO","something");

// 下面的定义是合法的,但应该避免这样做:(自定义常量不要以__开头)
// 也许将来有一天 PHP 会定义一个__FOO__的魔术常量
// 这样就会与你的代码相冲突
define("__FOO__","something");
?>
```

**Note**:在这里,字母指的是 a-z,A-Z,以及从 127 到 255(0x7f-0xff)的 ASCII 字符.

和 superglobals 一样,常量的范围是全局的.不用管作用区域就可以在脚本的任何地方访问常量.

### 语法
可以用 define() 函数来定义常量,在 PHP 5.3.0 以后,可以使用 const 关键字在类定义之外定义常量.一个常量一旦被定义,就不能再改变或者取消定义.
常量只能包含标量数据(boolean,integer,float 和 string).可以定义 resource 常量,但应尽量避免,因为会造成不可预料的结果.
可以简单的通过指定其名字来取得常量的值,与变量不同,*不应该*在常量前面加上 $ 符号.如果常量名是动态的,也可以用函数 constant() 来获取常量的值.用 get_defined_constants() 可以获得所有已定义的常量列表.

**Note**:常量和(全局)变量在不同的名字空间中.这意味着例如 TRUE 和 $TURE 是不同的.

常量和变量有如下不同:

* 常量前面没有美元符号($);
* 常量只能用 define() 函数定义,而不能通过赋值语句;
* 常量可以不用理会变量的作用域而在任何地方定义和访问;
* 常量一旦定义就不能被重新定义或者取消定义;
* 常量的值只能是标量.

Example #1 定义常量
```PHP
<?php
define("CONSTANT","Hello world.");
echo CONSTANT; // outputs "Hello world."
echo Constant; // 输出 "Constant" 并发出一个提示级别错误信息
?>
```

Example #2 使用关键字 const 定义常量
```PHP
<?php
// 以下代码在 PHP 5.3.0 后可以正常工作
const CONSTANT = 'Hello World';

echo CONSTANT;
?>
```

**Note**:和使用 define() 来定义常量相反的是,使用 const 关键字定义常量必须处于最顶端的作用区域,因为用此方法是在编译时定义的.这就意味着不能在函数内,循环内以及 if 语句之内用 const 来定义常量.

### 魔术常量
PHP 向它运行的任何脚本提供了大量的与定义常量.不过很多常量都是由不同的扩展库定义的,只有在加载了这些扩展库时才会出现,或者动态加载后,或者在编译时已经包括进去了.
有八个魔术常量它们的值随着它们在代码中的位置改变而改变.例如__LINE__的值就依赖于它在脚本中所处的行来决定.这些特殊的常量不区分大小写,如下:

Table 1. 几个 PHP 的"魔术常量"

|名称|说明|
|:--:|----|
|__LINE__|文件中的当前行号.|
|__FILE__|文件的完整路径和文件名.如果用在被包含文件中,则返回被包含的文件名.在 PHP 4.0.2 起,__FILE__总是包含一个绝对路径(如果是符号连接,则是解析后的绝对路径),而在此之前的版本有时会包含一个相对路径.|
|__DIR__|文件所在的目录.如果用在被包括文件中,则返回被包括的文件所在的目录.它等价于 dirname(__FILE__).除非是根目录,否则目录中名不包括末尾的斜杠.(PHP 5.3.0 中新增)|
|__FUNCTION__|函数名称(PHP 4.3.0 新增).自 PHP 5 起本常量返回该函数被定义时的名字(区别大小写).在 PHP 4 中该值总是小写字母的.|
|__CLASS__|类的名称(PHP 4.3.0 新加).自 PHP 5 起本常量返回该类被定义时的名字(区别大小写).在 PHP 4 中该值总是小写字母的.类名包括其被声明的作用区域(例如 FOO\Bar).注意自 PHP 5.4 起 __CLASS__ 对 trait 也起作用.当用在 trait 方法中时,__CLASS__ 是调用 trait 方法的类的名字.|
|__TRAIT__|Trait 的名字(PHP 5.4.0 新加).自 PHP 5.4 起此常量返回 trait 被定义时的名字(区分大小写).Trait 名包括其被声明的作用区域(例如 Foo\Bar).|
|__METHOD__|类的方法名(PHP 5.0.0 新加).返回该方法被定义时的名字(区分大小写).|
|__NAMESPACE__|当前命名空间的名称(区分大小写).此常量是在编译时定义的(PHP 5.3.0 新增).|

下面我将举例展示上述几个"魔术常量":

```PHP
<?php
echo __LINE__."\n";
echo __FILE__."\n";
echo __DIR__."\n";
function test()
{
    echo __FUNCTION__;
}
class Student{
    function say(){
        echo __CLASS__;
    }
    function magic(){
        echo __METHOD__;
    }
}
echo __NAMESPACE__;
$a = new Student();
$a -> say();
$a -> magic();
?>
```
![magic1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/magic1.png)

## 表达式

表达式是 PHP 最重要的基石.在 PHP 中,几乎所写的任何东西都是一个表达式.简单但却最精确的定义一个表达式的方式就是"任何有值的东西".
最基本的表达式形式是常量和变量.像"$a=5".
稍微复杂的表达式例子就是函数.例如,考虑下面的函数:
```PHP
<?php
function foo()
{
    return 5;
}
?>
```
函数表达式的值即为它们的返回值.
下面的例子有助于理解前、后递增和表达式:
```PHP
<?php
function double($i)
{
    return $i*2;
}
$b = $a = 5; /* assign the value five into the variable $a and $b */
$c = $a++; /* post-increment, assign original value of $a (5) to $c */
$e = $d ++$b; /*pre-increment, assign the incremented value of $b (6) to $d and $e */
/* at this point, both $d and $e are equal to 6 */
$f = double($d++); /* assign twice the value of $d before the increment, 2*6 = 12 to $f */
$g = double(++$e); /* assign twice the value of $e after the increment, 2*7 = 14 to $g */
$h = $g += 10; /* first, $g is incremented by 10 and ends with the value of 24. the value of the assignment (24) is then assigned into $h, and $h ends with the value of 24 as well. */
?>
```

## 运算符
运算符是可以通过给出的一或多个值(用编程行话来说,表达式)来产生另一个值(因为整个结构成为一个表达式)的东西.
运算符可按照其能接受几个值来分组.分为以下几类:

* 一元运算符
1. !
2. ++
3. ...
* 二元运算符
1. +
2. -
3. ...
* 三元运算符
1. ?:
2. ...

### 运算符优先级
运算符优先级指定了两个表达式绑定得有多"紧密".例如,表达式 1+5*3 的结果是 16 而不是 18 是因为乘号("*")的优先级比加号("+")高.必要时可以用括号来强制改变优先级.例如:(1+5)*3 的值为 18.
如果运算符优先级相同,那运算符的结合方向决定了该如何运算.例如,"-"是左联的,那么 1-2-3 就等同于(1-2)-3 并且结果是 -4.另外一方面,"="是右联的,所以 $a=$b=$c 等同于 $a=($b=$c).
没有结合的相同优先级的运算符不能连在一起使用,例如 1<2>1 在 PHP 是不合法的.但另外一方面表达式 1<=1==1 是合法的,因为 == 的优先级低于 <=.
Table 1.运算符优先级

|结合方向|运算符|附加信息|
|:------:|------|--------|
|无|clone new|clone 和 new|
|左|[|array()|
|右|**|算术运算符|
|右|++ -- ~ (int)(float)(string)(array)(object)(bool)@|类型和递增/递减|
|无|instanceof|类型|
|右|!|逻辑运算符|
|左|*/%|算术运算符|
|左|+ - .|算数运算符和字符运算符|
|左|<< >>|位运算符|
|无|< <= > >=|比较运算符|
|无|== != === !== <> <=>|比较运算符|
|左|&|位运算符|
|左|^|位运算符|
|左|/|位运算符|
|左|&&|逻辑运算符|
|左|//|逻辑运算符|
|左|??|比较运算符|
|左|?:|ternary|
|right|= += -= *= **= /= .= %= &= /= ^= <<= >>=|赋值运算符|
|左|and|逻辑运算符|
|左|xor|逻辑运算符|
|左|or|逻辑运算符|

Example #1 结合方向
```PHP
<?php
$a = 3 * 3 % 5; // (3*3) % 5 = 4
// ternary operator associativity differs from C/C++
$a = true ? 0 : true ? 1 : 2; // (true ? 0 : true) ? 1 : 2 = 2

$a = 1;
$b = 2;
$a = $b += 3; // $a = ($b += 3) -> $a = 5, $b = 5
?>
```
Example #2 Undefined order of evaluation
```PHP
<?php
$a = 1;
echo $a + $a++; // may print either 2 or 3
$i = 1;
$array[$i] = $i++; // may set either index 1 or 2
?>
```
**Note**: 尽管 = 比其它大多数的运算符的优先级低,PHP 仍旧允许类似如下的表达式:if(!$a=foo()),在此例中 foo() 的返回值被赋给了 $a.

### 算数运算符
Table 1.算数运算符

|例子|名称|结果|
|:--:|----|----|
|-$a|取反|$a 的负值.|
|$a+$b|加法|$a 和 $b 的和.|
|$a-$b|减法|$a 和 $b 的差.|
|$a*$b|乘法|$a 和 $b 的积.|
|$a/$b|除法|$a 和 $b 的商.|
|$a%$b|取模|$a 和 $b 的余数.|
|$a**$b|Exponentiation|Result of raising $a to the $b'th power.Introduced in PHP 5.6.|

除法运算符总是返回浮点数.只有在下列情况下例外:两个操作数都是整数(或字符串转换成的整数)并且正好能整除,这时它返回一个整数.
取模运算符的操作数在运算之前都会转换成整数(除去小数部分).
取模运算符 % 的结果和被除数的符号(正负号)相同.即 $a % $b 的结果和 $a 的结果相同.例如:
```PHP
<?php
echo (5%3)."\n"; // prints 2
echo (5%-3)."\n"; // prints 2
echo (-5%3)."\n"; // prints -2
echo (-5%-3)."\n"; // prints -2
?>
```

### 赋值运算符

基本的赋值运算符是"=".一开始可能会以为它是"等于",其实不是的.它实际上意味着把右边表达式的值赋给左边的运算数.
在基本赋值运算符之外,还有适合于所有二元算术,数组集合和字符串运算符的"组合运算符",这样可以在一个表达式中使用它的值并把表达式的结果赋给它,例如:
```PHP
<?php
$a = 3;
$a += 5; // sets $a to 8, as if we had said: $a = $a + 5;
$b = "Hello";
$b .="There!"; // set $b to "Hello There!", just like $b = $b."There";
?>
```
赋值运算的原变量的值拷贝到新变量中(传值赋值),所以改变其中一个并不影响另一个.这也适合于在密集循环中拷贝一些值例如大数组.
在 PHP 中普通的传值赋值行为有个例外就是碰到对象 object 时,在 PHP 5 中是以引用赋值的,除非明确使用了 clone 关键字来拷贝.

#### 引用赋值
PHP 支持引用赋值,使用"$var = &$othervar;"语法.引用赋值意味着两个变量指向了同一个数据,没有拷贝任何东西.

Example #1 引用赋值
```PHP
<?php
$a = 3;
$b = &$a; // $b 是 $a 的引用

print "$a\n"; // 输出3
print "$b\n"; // 输出3

$a = 4; // 修改 $a

print "$a\n"; // 输出4
print "$b\n"; // 也输出4,因为 $b 是 $a 的引用,因此也被改变
?>
```
自 PHP 起,new 运算符自动返回一个引用,因此再对 new 的结果进行引用赋值在 PHP 5.3 以及以后版本中会发出一条 E_DEPRECATED 错误信息,在之前版本会发出一条 E_STRICT 错误信息.

例如以下代码将产生警告:
```PHP
<?php
class C {}

/* The following line generates the following error message:
 * Deprecated: Assigning the return value of new by reference is deprecated in ...
 */
 $o = &new C;
 ?>
```

### 位运算符
位运算符允许对整型数中指定的位进行求值和操作.

Table 1.位运算符

|例子|名称|结果|
|----|----|----|
|$a & $b|And(按位与)|将把 $a 和 $b 中都为 1 的位设为 1.|
|$a / $b|Or(按位或)|将把 $a 和 $b 中任何一个为 1 的位设为 1.|
|$a ^ $b|Xor(按位异或)|将把 $a 和 $b 中一个为 1 另一个为 0 的位设为 1.|
|~ $a|Not(按位取反)|将 $a 中为 0 的位设为 1,反之亦然.|
|$a << $b|Shift left(左移)|将 $a 中的位向左移动 $b 次(每一次移动都表示"乘以 2")|
|$a >> $b|Shift right(右移)|将 $a 中的位向右移动 $b 次(每一次移动都表示"除以 2")|

Example #1 整数的 AND,OR 和 XOR 位运算符
```PHP
<?php
/*
 * Ignore the top section,
 * it is just formatting to make output clearer.
 */

$format = '(%1$2d = %1$04b) = (%2$2d = %2$04b)'
        . ' %3$s (%4$2d = %4$04b)' . "\n";

echo <<<EOH
 -----------    ----------  --  ----------
 result         value       op  test
 -----------    ----------  --  ----------
EOH;

/*
 * Here are the examples.
 */

$values = array(0,1,2,4,8);
$test = 1 + 4;

echo "\n Bitwise AND \n";
foreach ($values as $value){
    $result = $value & $test;
    printf($format,$result,$value,'&',$test);
}

echo "\n Bitwise Inclusive OR \n";
foreach ($values as $value){
    $result = $value | $test;
    printf($format,$result,$value,'|',$test);
}

echo "\n Bitwise Exclusive OR (XOR) \n";
foreach ($values as $value){
    $result = $value ^ $test;
    printf($format,$result,$value,'^',$test);
}
?>
```
以上例程会输出:
![Bitwise1.png]()

### 比较运算符

比较运算符,如同它们名称所暗示的,允许对两个值进行比较.

Table 1.比较运算符

|例子|名称|结果|
|:--:|----|----|
|$a==$b|等于|TRUE,如果类型转换后 $a 等于 $b.|
|$a===$b|全等|TRUE,如果 $a 等于 $b,并且它们的类型也相同.|
|$a!=$b|不等|TRUE,如果类型转换后 $a 不等于 $b.|
|$a<>$b|不等|TRUE,如果类型转换后 $a 不等于 $b.|
|$a!==$b|不全等|TRUE,如果 $a 不等于 $b,或者它们的类型不同.|
|$a<$b|小于|TRUE,如果 $a 严格小于 $b.|
|$a>$b|大于|TRUE,如果 $a 严格大于 $b.|
|$a<=$b|小于等于|TRUE,如果 $a 小于或者等于 $b.|
|$a>=$b|大于等于|TRUE,如果 $a 大于或者等于 $b.|
|$a<=>$b|结合比较运算|当 $a小于、等于、大于 than $b 时分别返回一个小于、等于、大于0的 integer 值.PHP 7 开始提供.|
|$a??$b??$c|NULL 合并操作符|从左往右第一个存在且不为 NULL 的操作数.如果都没有定义且不为 NULL ,则返回 NULL.PHP 7 开始提供.|

**Note**:如果比较一个数字和字符串或者比较涉及到数字内容的字符串,则字符串会被转换为数值并且比较按照数值来进行.当用 === 或 !== 进行比较时则不进行类型转换,因为此时类型和数值都要比对.
```PHP
<?php
var_dump(0 == "a"); // 0 == 0 -> true
var_dump("1" == "01"); // 1 == 1 -> true
var_dump("10" == "1e1"); // 10 == 10 -> true
var_dump(100 == "1e2"); // 100 == 100 -> true

switch("a"){
    case 0:
        echo "0";
        break;
    case "a": // never reached because "a" is already matched with 0
    echo "a";
    break;
}
?>
```

```PHP
<?php
// Integers
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1

// Floats
echo 1.5 <=> 1.5; // 0
echo 1.5 <=> 2.5; // -1
echo 2.5 <=> 1.5; // 1

// Strings
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; //1

echo "a" <=> "aa"; // -1
echo "zz" <=> "aa"; // 1

// Arrays
echo [] <=> []; // 0
echo [1,2,3] <=> [1,2,3]; // 0
echo [1,2,3] <=> []; // 1
echo [1,2,3] <=> [1,2,1]; // 1
echo [1,2,3] <=> [1,2,4]; // -1

// Objects
$a = (object)["a" => "b"];
$b = (object)["a" => "b"];
echo $a <=> $b; // -1

$a = (object)["a" => "c"];
$b = (object)["a" => "b"];
echo $a <=> $b; // 1

// only values are compared
$a = (object)["a" => "b"];
$b = (object)["b" => "b"];
echo $a <=> $b; // 1
?>
```
Table 2.比较多种类型

|运算数 1 类型|运算数 2 类型|结果|
|:-----------:|:-----------:|----|
|null 或 string|string|将 NULL 转换为"",进行数字或词汇比较.|
|bool 或 null|任何其它类型|转换为 bool,FALSE<TRUE>|
|object|object|内置类可以定义自己的比较,不同类不能比较,相同类和数组同样方式比较属性(PHP 4 中),PHP 5 有其自己的说明.|
|string,resource 或 number|string,resource 或 number|将字符串和资源转换成数字,按普通数学比较.|
|array|array|具有较少成员的数组较小,如果运算数 1 中的键不存在于运算数 2 中则数组无法比较,否则挨个值比较.|
|object|任何其它类型|object 总是更大|
|array|任何其它类型|array 总是更大|

Example #1 标准数组比较代码
```PHP
<?php
// 数组是用标准比较运算符这样比较的
function standard_array_compare($op1,$op2)
{
    if(count(%op1) < count($op2)){
        return -1; // $op1 < $op2
    } elseif(count($op1) > count($op2)){
        return 1; // $op1 > $op2
    }
    foreach ($op1 as $key => $val){
        if(!array_key_exists($key,$op2)){
            return null; // uncomparable
        } elseif($val < $op2[key]){
            return -1;
        } elseif($val > $op2[$key]){
            return 1;
        }
    }
    return 0; // $op1 == $op2
}
?>
```

Example #2 Transcription of standard array comparison
```PHP
<?php
// Arrays are compared like this with standard comparison operators
function standard_array_compare($op1,$op2)
{
    if(count($op1)<count($op2)){
        return -1; // $op1<$op2
    } elseif(count($op1)>count($op2)){
        return 1; // $op1>$op2
    }
    foreach($op1 as $key=>$val){
        if(!array_key_exists($key,$op2)){
            return null; // uncomparable
        } elseif($val<$op2[$key]){
            return -1;
        } elseif($val<$op2[$key]){
            return 1;
        }
    }
    return 0; // $op1 == $op2
}
?>
```
**Warning**:比较浮点数,由于浮点数 float 的内部表达方式,不应比较两个浮点数 float 是否相等.

#### 三元运算符
另一个条件运算符是 "?:"(或三元)运算符.

Example #3 赋默认值
```PHP
<?php
// Example usage for: Ternary Operator
$action = (empty($_POST['action']))?'default':$_POST['action'];

// The above is identical to this if/else statement
if(empty($_POST['action'])){
    $action = 'default';
} else{
    $action = $_POST['action'];
}
?>
```
表达式 (expr1)?(expr2):(expr3) 在 expr1 求值为 TRUE 时的值为 expr2,在 expr1 求值为 FALSE 时的值为 expr3.
**Note**:注意三元运算符是个语句,因此其求值不是变量,而是语句的结果.如果想通过引用返回一个变量这点就很重要.在一个通过引用返回的函数中语句 return $var==42?$a:$b;将不起作用,以后的 PHP 版本会为此发出一条警告.

Example #4 不清晰的三元运算符行为
```PHP
<?php
// 乍看起来下面的输出是 'true'
echo(true?'true':false?'t':'f');

// 然而,上面语句的实际输出是 't',因为三元运算符是从左往右计算的

// 下面是与上面等价的语句,但更清晰
echo((true?'true':'false')?'t':'f');
?>
```

### 错误控制运算符
PHP 支持一个错误控制运算符:@.当将其放置在一个 PHP 表达式之前,该表达式可能产生的任何错误信息都被忽略掉.

```PHP
<?php
/* Intentional file error */
$my_file=@file('non_existent_file')or die("Failed opening file:error was '$php_errormsg'");

// this works for any expression,not just functions:
$value=@$cache[$key];
// will not issue a notice if the index $key doesn't exist.
?>
```
**Note**:@ 运算符只对表达式有效.对新手来说一个简单的规则就是:如果能从某处得到值,就能在它前面加上 @ 运算符.例如,可以把它放在变量,函数和 include 调用,常量,等等之前.不能把它放在函数或类的定义之前,也不能用于条件结构例如 if 和 foreach 等.

**Warning** 目前的"@"错误控制运算符前缀甚至使导致脚本终止的严重错误的错误报告也失效.这意味着如果在某个不存在或者敲错了字母的函数调用前用了"@"来抑制错误信息,那脚本会没有任何迹象显示原因而死在那里.

### 执行运算符
PHP 支持一个执行运算符:反引号(``).注意这不是单引号!PHP 将尝试将反引号中的内容作为 shell 命令来执行,并将其输出信息返回(即,可以赋给一个变量而不是简单地丢弃到标准输出).使用反引号运算符"`"的效果与函数 shell_exec() 相同.

```PHP
<?php
$output=`ls -la`;
echo "<pre>$output</pre>";
?>
```
**Note**:反引号运算符激活了安全模式或者关闭了

### 递增/递减运算符
PHP 支持 C 风格的前/后递增与递减运算符.

**Note**:递增/递减运算符不影响布尔值.递减 NULL 值也没有效果,但是递增 NULL 的结果是1.

Table.1 递增/递减运算符

|例子|名称|效果|
|:--:|:--:|----|
|++$a|前加|$a 的值加一,然后返回 $a.|
|$a++|后加|返回 $a,然后将 $a 的值加一.|
|--$a|前减|$a 的值减一,然后返回 $a.|
|$a--|后减|返回 $a,然后将 $a 的值减一.|

一个简单的示例脚本:
```PHP
<?php
echo "<h3>Postincrement</h3>";
$a = 5;
echo "Should be 5:".$a++."<br>\n";
echo "Shoule be 6:".$a."<br>\n";
echo "<h3>Preincrement</h3>";
$a = 5;
echo "Should be 6:".++$a."<br>\n";
echo "Shoule be 6:".$a."<br>\n";
echo "<h3>Postdecrement</h3>";
$a = 5;
echo "Should be 5:".$a--."<br>\n";
echo "Shoule be 4:".$a."<br>\n";
echo "<h3>Predecrement</h3>";
$a = 5;
echo "Should be 4:".--$a."<br>\n";
echo "Should be 4:".$a."<br>\n";
?>
```
在处理字符变量的算数运算时,PHP 沿袭了 Perl 的习惯,而非 C 的.例如,在 Perl 中 $a='Z';$a++;将把 $a 变成'AA',而在 C 中,a='Z';a++; 将把 a 变成'\['('Z'的 ASCII 值是90,'\['的 ASCII 值是91).注意字符串变量只能递增,不能递减,并且只支持纯字母(a-z 和 A-Z).递增/递减其它字符变量则无效,原字符串没有变化.

Example #1 涉及字符变量的算数运算
```PHP
<?php
echo '== Alphabets =='.PHP_EOL;
$s = 'W';
for ($n=0;$n<6;$n++){
    echo ++$s.PHP_EOL;
}
// Digit characters behave differently
echo '== Digits =='.PHP_EOL;
$d = 'A8';
for ($n=0;$n<6;$n++){
    echo ++$d.PHP_EOL;
}
$d = 'A08';
for ($n=0;$n<6;$n++){
    echo ++$d.PHP_EOL;
}
?>
```
以上例程会输出:

![increment1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/increment1.png)

递增和递减布尔值没有效果.
References:

* [Official Website](http://php.net)
* [PSR](http://www.php-fig.org/)
* [PHP100](http://www.php100.com/)

