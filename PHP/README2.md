# PHP

由于 README.md 文件行数过多,所以从 PHP 流程控制开始以一个新的文件说明.

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

### declare

declare 结构用来设定一段代码的执行指令.declare 的语法和其它流程控制结构相似:
```PHP
declare(directive){

    statement
}
```
directive 部分允许设定 declare 代码段的行为.目前只认识两个指令:ticks 以及 encoding.

declare 代码段中的 statement 部分将被执行——怎样执行以及执行中有什么副作用出现取决于 directive 中设定的指令.

declare 结构也可用于全局范围,影响到其后的所有代码(但如果有 declare 结构的文件被其它文件包含,则对包含它的父文件不起作用).
```PHP
<?php
// these are the same:

// you can use this:
declare(ticks=1){
    // entire script here
}

// or you can use this:
declare(ticks=1);
// entire script here
?>
```

#### Ticks
Tick (时钟周期)是一个在 declare 代码段中解释器每执行 N 条可计时的低级语句就会发生的事件.N 的值是在 declare 中的 directive 部分用 ticks=N 来指定的.
不是所有语句都可计时.通常条件表达式和参数表达式都不可计时.
在每个 tick 中出现的事件是由 register_tick_function() 来指定的.更多细节见下面的例子.注意每个 tick 中可以出现多个事件.

Example #1 Tick 的用法示例
```PHP
<?php
declare(ticks=1);

// A function called on each tick event
function tick_handler()
{
    echo "tick_handler() called\n";
}

register_tick_function('tick_handler');

$a = 1;

if($a > 0){
    $a += 2;
    print($a);
}
?>
```
Example #2 Ticks 的用法示例

```PHP
<?php

function tick_handler()
{
    echo "tick_handler() called\n";
}
$a = 1;
tick_handler();
if($a > 0){
    $a += 2;
    tick_handler();
    print($a);
    tick_handler();
}
tick_handler();
?>
```

#### Encoding
可以用 encoding 指令来对每段脚本指定其编码方式.

Example #3 对脚本指定编码方式
```PHP
<?php
declare(encoding='ISO-8859-1');
// code here
?>
```

### return
如果在一个函数中调用 return 语句,将立即结束此函数的执行并将它的参数作为函数的值返回.return 也会终止 eval() 语句或者脚本文件的执行.

如果在全局范围中调用,则当前脚本文件中止运行.如果当前脚本文件是被 include 的或者 require 的,则控制交回调用文件.此外,如果当前脚本是被 include 的,则 return 的值会被当作 include 调用的返回值.如果在主脚本文件调用 return,则脚本中止运行.如果当前脚本文件是在 php.ini 中的配置选项 auto_prepend_file 或者 auto_append_file 所指定的,则此脚本文件中止运行.

**Note**:注意既然 return 是语言结构而不是函数,因此其参数没有必要用括号将其括起来.通常都不用括号,实际上也应该不用,这样可以降低 PHP 的负担.

**Note**:如果没有提供参数,则一定不能用括号,此时返回 NULL.如果调用 return 时加上了括号却又没有参数会导致解析错误.

**Note**:当用引用返回值时*永远不要*使用括号,这样行不通.只能通过引用返回变量,而不是语句的结果.如果使用 return($a);时其实不是返回一个变量,而是表达式($a)的值(当然,此时该值也正是 $a 的值).

### require
require 和 include 几乎完全一样,除了处理失败的方式不同之外.require 在出错时产生 E_COMPILE_ERROR 级别的错误.换句话说将导致脚本中止而 include 只产生警告(E_WARNING),脚本会继续运行.

### include
include 语句包含并运行指定文件.
以下文档也适用于 require.
被包含文件先按参数给出的路径寻找,如果没有给出目录(只有文件名)时则按照 include_path 指定的目录寻找.如果在 include_path 下没找到该文件则 include 最后才在调用脚本文件所在的目录和当前工作目录下寻找.如果最后仍未找到文件则 include 结构会发出一条警告;这一点和 require 不同,后者会发出一个致命错误.

如果定义了路径——不管是绝对路径(在 Windows 下以盘符或者\开头,在 Unix/Linux 下以/开头)还是当前目录的相对路径(以.或者..开头)——include_path 都会被完全忽略.例如一个文件以../开头,则解析器会在当前目录的父目录下寻找该文件.

当一个文件被包含时,其中所包含的代码继承了 include 所在行的变量范围.从该处开始,调用文件在该行处可用的任何变量在被调用的文件中也都可用.不过所有在包含文件中定义的函数和类都具有全局作用域.

Example #1 基本的 include 例子
```PHP
vars.php
<?php
$coler = 'green';
$fruit = 'apple';
?>
test.php
<?php
echo "A $color $fruit"; // A
include 'vars.php';
echo "A $color $fruit"; // A green apple
?>
```
如果 include 出现于调用文件中的一个函数里,则被调用的文件中所包含的所有代码将表现得如同它们是在该函数内部定义的一样.所以它将遵循该函数的变量范围.此规则的一个例外是魔术常量,他们是在发生包含之前就已被解析器处理的.

Example #2 函数中的包含
```PHP
<?php
function foo()
{
    global $color;
    include 'vars.php';
    echo "A $color $fruit";
}
/* vars.php is in the scope of foo() so *
 * $fruit is NOT available outside of this *
 * scope. $color is because we declared it *
 * as global. */

foo(); // A green apple
echo "A $color $fruit"; // A green
?>
```
当一个文件被包含时,语法解析器在目标文件的开头脱离 PHP 模式并进入 HTML 模式,到文件结尾处恢复.由于此原因,目标文件中需要作为 PHP 代码执行的任何代码都必须被包括在有效的 PHP 起始和结束标记之中.

Example #3 通过 HTTP 进行的 include
```PHP
<?php
/* This example assumes that www.example.com is configured to parse .php *
 * files and not .txt files. Also,'Works' here means that the variables *
 * $foo and $bar are available within the included file. */
 
 // Won't work;file.txt wasn't handled by www.example.com as PHP
 include'http://www.example.com/file.txt?foo=1&bar=2;

// Won't work;looks for a file named 'file.php?foo=1&bar-2' on the
// local filesystem.
include 'file.php?foo=1&bar=2';

// Works.
include 'http://www.example.com/file.php?foo=1&bar=2;

$foo = 1;
$bar = 2;
include 'file.txt'; // Works.
include 'file.php'; // Works.
?>
```
因为 include 是一个特殊的语言结构,其参数不需要括号.在比较其返回值时要注意.

Example #4 比较 include 的返回值
```PHP
<?php
// won't work, evaluated as include(('vars.php') == 'OK'),i.e. include('')
if (include('vars.php') == 'OK') {
    echo 'OK';
}
// works
if ((include 'vars.php') == 'OK') {
    echo 'OK';
}
?>
```
Example #5 include 和 return 语句
```PHP
return.php
<?php
$var = 'PHP';
return $var;
?>

noreturn.php
<?php
$var = 'PHP';
?>

testreturns.php
<?php
$foo = include 'return.php';
echo $foo; // prints 'PHP'
$var = include 'noreturn.php';
echo $bar; // prints 1
?>
```
$bar 的值为1是因为 include 成功运行了.注意以上例子中的区别.第一个在被包含的文件中用了 return 而另一个没有.如果文件不能被包含,则返回 FALSE 并发出一个 E_WARNING 警告.

如果在包含文件中定义有函数,这些函数不管是在 return 之前还是之后定义的,都可以独立在主文件中使用.如果文件被包含两次,PHP 5发出致命错误因为函数已经被定义,但是 PHP 4不会对在 return 之后定义的函数报错.推荐使用 include_once 而不是检查文件是否包含并在包含文件中有条件返回.

另一个将 PHP 文件"包含"到一个变量中的方法是用输出控制函数结合 include 来捕获其输出,例如:

Example #6 使用输出缓冲来将 PHP 文件包含入一个字符串
```PHP
<?php
$string = get_include_contents('somefile.php');
function get_include_contents($filename){
    if(is_file($filename)){
        ob_start();
        include $filename;
        $contents = ob_get_contents();
        ob_end_clean();
        return $contents;
    }
    return false;
}
?>
```
References:
* [Official Website](http://php.net)
* [PSR](http://www.php-fig.org/)
* [PHP100](http://www.php100.com/)

