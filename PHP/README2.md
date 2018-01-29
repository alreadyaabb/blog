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

### require_once
require_once 语句和 require 语句完全相同,唯一区别是 PHP 会检查该文件是否已经被包含过,如果是则不会再次包含.

### include_once
include_once 语句在脚本执行期间包含并运行指定文件.此行为和 include 语句类似,唯一区别是如果该文件中已经被包含过,则不会再次包含.如同此语句名字暗示的那样,只会包含一次.

include_once 可以用于在脚本执行期间同一个文件有可能被包含超过一次的情况下,想确保它只被包含一次以避免函数重定义,变量重新赋值等问题.

**Note**:在 PHP 4中,_once 的行为在不区分大小写字母的操作系统中有所不同,例如:

Example #1 include_once 在 PHP 4运行于不区分大小写的操作系统中
```PHP
include_once "a.php"; // 这将包含 a.php
include_once "A.php"; // 这将再次包含 a.php!(仅 PHP 4)
?>
```

### goto
goto 操作符可以用来跳转到程序中的另一个位置.该目标位置可以用目标名称加上冒号来标记,而跳转指令是 goto 之后接上目标位置的标记.PHP 中的 goto 有一定限制,目标位置只能位于同一个文件和作用域,也就是说无法跳出一个函数或类方法,也无法跳入到另一个函数.也无法跳入到任何循环或者 switch 结构中.可以跳出循环或者 swtich ,通常的用法是用 goto 代替多层的 break.

Example #1 goto 示例
```PHP
<?php
goto a;
echo 'Foo';

a:
echo 'Bar';
?>
```
以上例程会输出:

![goto1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/goto1.png)

Example #2 goto 跳出循环示例
```PHP
<?php
for($i=0,$j=50;$i<100;$i++){
    while($j--){
        if($j==17) goto end;
    }
}
echo "i = $i";
end:
echo 'j hit 17';
?>
```
以上例程会输出:

![goto2.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/goto2.png)

Example #3 以下写法无效
```PHP
<?php
goto loop;
for($i=0,$j=50;$i<100;$i++){
    while($j--){
        loop:
    }
}
echo "$i = $i";
?>
```
以上例程会输出:

![goto3.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/goto3.png)

## 函数

### 用户自定义函数

一个函数可由以下的语法来定义:

Example #1 展示函数用途的伪代码
```PHP
<?php
function foo($arg_1,$arg_2,/* ..., */ $arg_n)
{
    echo "Example function.\n";
    return $retval;
}
?>
```
任何有效的 PHP 代码都有可能出现在函数内部,甚至包括其它函数和类定义.

函数名和 PHP 中的其它标识符命名规则相同.有效的函数名以字母或下划线打头,后面跟字母,数字或下划线.可以用正则表达式表示为:[a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*.

函数无需在调用之前被定义,*除非*是下面两个例子中函数是有条件被定义时.

当一个函数是有条件被定义时,必须在调用函数*之前*定义.

Example #2 有条件的函数
```PHP
<?php
$makefoo = true;
/* 不能在此处调用foo()函数,
   因为它还不存在,但可以调用bar()函数. */
bar();
if($makefoo){
    function foo()
    {
        echo "I don't exist until program execution reaches me.\n";
    }
}
/* 现在可以安全调用函数 foo()了,
   因为 $makefoo 值为真 */
if($makefoo)foo();
function bar()
{
    echo "I exist immediately upon program start.\n";
}
?>
```
Example #3 函数中的函数
```PHP
<?php
function foo()
{
    function bar()
    {
        echo "I don't exist untill foo() is called.\n";
    }
}
/* 现在还不能调用 bar() 函数,因为它还不存在 */
foo();
/* 现在可以调用 bar() 函数了,因为 foo() 函数
   的执行使得 bar() 函数变为已定义的函数 */
bar();
?>
```
PHP 中的所有函数和类都具有全局作用域,可以定义在一个函数之内而在之外调用,反之亦然.

PHP 不支持函数重载,也不可能取消定义或者重定义已声明的函数.

**Note**:函数名是大小写无关的,不过在调用函数的时候,使用其在定义时相同的形式是个好习惯.

在 PHP 中可以调用递归函数.

Example #4 递归函数
```PHP
<?php
function recursion($a)
{
    if($a<20){
        echo "$a\n";
        recursion($a + 1);
    }
}
?>
```
**Note**:但是要避免递归函数/方法调用超过100-200层,因为可能会使堆栈崩溃从而使当前脚本终止.无限递归可视为编程错误.

### 函数的参数
通过参数列表可以传递信息到函数,即以逗号作为分隔符的表达式列表.参数是从左向右求值的.

PHP 支持按值传递参数(默认),通过引用传递参数以及默认参数.也支持可变长度参数列表.

Example #1 向函数传递数组
```PHP
<?php
function takes_array($input)
{
    echo "$input[0] + $input[1] = ",$input[0]+$input[1];
}
?>
```
#### 通过引用传递参数
默认情况下,函数参数通过值传递(因而即使在函数内部改变参数的值,它并不会改变函数外部的值).如果希望允许函数修改它的参数值,必须通过引用传递参数.

如果想要函数的一个参数总是通过引用传递,可以在函数定义中该参数的前面加上符号&:

Example #2 用引用传递函数参数
```PHP
<?php
function add_some_extra(&$string)
{
    $string .= 'and something extra.';
}
$str = 'This is a string, ';
add_some_extra($str);
echo $str; // outputs 'This is a string, and something extra.'
?>
```

#### 默认参数的值
函数可以定义 C++ 风格的标量参数默认值,如下所示:

Example #3 在函数中使用默认参数
```PHP
<?php
function makecoffee($type = "cappuccino")
{
    return "Making a cup of $type.\n";
}
echo makecoffee();
echo makecoffee(null);
echo makecoffee("espresso");
?>
```
以上例程会输出:

![arguments1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/arguments1.png)

PHP 还允许使用数组 array 和特殊类型 NULL 作为默认参数,例如:

Example #4 使用非标量类型作为默认参数
```PHP
<?php
function makecoffee($types = array("cappuccion"),$coffeeMaker = NULL)
{
    $device = is_null($coffeeMaker) ? "hands" : $coffeeMaker;
    return "Making a cup of ".join(",",$types),"with $device.\n";
}
echo makecoffee();
echo makecoffee(array("cappuccion","lavazza"),"teapot");
?>
```
以上例程输出为:

![arguments2.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/arguments2.png)

默认值必须是常量表达式,不能是诸如变量,类成员,或者函数调用等.

注意当使用默认参数时,任何默认参数必须放在任何非默认参数的右侧;否则,函数将不会按照预期的情况工作.考虑下面的代码片段:

Example #5 函数默认参数的不正确用法
```PHP
<?php
function makeyogurt($type = "acidophilus",$flavour)
{
    return "Making a bowl of $type $flavour.\n";
}
echo makeyogurt("raspberry"); // won't work as expected
?>
```
以上例程会输出:

![arguments3.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/arguments3.png)

现在,比较上面的例子和这个例子:

Example #6 函数默认参数正确的用法
```PHP
<?php
function makeyogurt($flavour,$type = "acidophilus")
{
    return "Making a bowl of $type $flavour.\n";
}
echo makeyogurt("raspberry"); // works as expected
?>
```
以上例程会输出:

![argument4.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/argument4.png)

#### 类型声明
类型声明允许函数在调用时要求参数为特定类型.如果给出的值类型不对,那么将会产生一个错误:在 PHP 5中,这将是一个可恢复的致命错误,而在 PHP 7中将会抛出一个 TypeError 异常.

为了指定一个类型声明,类型应该加到参数名前.这个声明可以通过将参数的默认值设为 NULL 来实现允许传递 NULL.

##### Valid types

|Type|Description|Minimum PHP version|
|----|-----------|-------------------|
|Class/interface name|The parameter must be an instanceof the given class or interface name.|PHP 5.0.0|
|self|The parameter must be an instanceof the same class as the one the method is defined on.This can only be used on class and instance methods.|PHP 5.0.0|
|array|The parameter must be an array.|PHP 5.1.0|
|callable|The parameter must be a valid callable.|PHP 5.4.0|
|bool|The parameter must be a boolean value.|PHP 7.0.0|
|float|The parameter must be a floating point number.|PHP 7.0.0|
|int|The parameter must be an integer.|PHP 7.0.0|
|string|The parameter must be a string.|PHP 7.0.0|

##### 范例

Example #7 基本类的声明
```PHP
<?php
class C {}
class D extends C {}

// This doesn't extend C.
class E {}

function f(C $c) {
    echo get_class($c)."\n";
}

f(new C);
f(new D);
f(new E);
?>
```

以上例程会输出:

![argument4.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/argument5.png)

Example #8 基本接口的声明
```PHP
<?php
interface I
{
    public function f();
}
class C implements I
{
    public function f() {}
}

// This doesn't implement I.
class E{}

function f(I $i)
{
    echo get_class($i)."\n";
}

f(new C);
f(new E);
?>
```
以上例程会输出:

![arguments6.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/arguments6.png)

Example #9 空类型的声明
```PHP
<?php
class C {}

function f(C $c = null)
{
    var_dump($c);
}

f(new C);
f(null);
?>
```
以上例程会输出:

![arguments7.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/arguments7.png)

##### 严格类型

默认情况下,如果能做到的话,PHP 将会强迫错误类型的值转为函数期望的标量类型.例如,一个函数的一个参数期望是 string,但传入的是 integer,最终函数得到的将会是一个 string 类型的值.

可以基于每一个文件开启严格模式.在严格模式中,只有一个与类型声明完全相符的变量才会被接受,否则将会抛出一个 TypeError.唯一的一个例外是可以将 integer 传给一个期望 float 的函数.

### 返回值

值通过使用可选的返回语句返回.可以返回包括数组和对象的任意类型.返回语句会立即中止函数的运行,并且将控制权交给调用该函数的代码行.

> **Note**:
> 如果省略了 return,则返回值为 NULL.

#### return 的使用

Example #1 return 的使用
```PHP
<?php
function square($num)
{
    return $num * $num;
}
echo square(4); // outputs '16'.
?>
```
函数不能返回多个值,但可以通过返回一个数组来得到类似的效果.

Example #2 返回一个数组以得到多个返回值
```PHP
<?php
function small_numbers()
{
    return array(0,1,2);
}
list($zero,$one,$two) = small_numbers();
?>
```
从函数返回一个引用,必须在函数声明和指派返回值给一个变量时都使用引用运算符 &:

Example #3 从函数返回一个引用
```PHP
<?php
function &return_reference()
{
    return $someref;
}
$newref=&return_reference();
?>
```

#### Return 的类型声明

Example #4 Basic return type declaration
```PHP
<?php
function sum($a,$b):float{
    return $a + $b;
}

// Note that a float will be returned.
var_dump(sum(1,2));
?>
```
以上例程会输出:

![f_r1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/f_r1.png)

Example #5 Strict mode in action
```PHP
<?php
declare(strict_types=1)

function sum($a,$b):int{
    return $a + $b;
}
var_dump(sum(1,2));
var_dump(sum(1,2.5));
?>
```
以上例程会输出:

![f_r2.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/f_r2.png)

Example #6 Returning an object
```PHP
<?php
class C{}

function getC():C{
    return new C;
}

var_dump(getC());
?>
```
以上例程会输出:

![f_r3.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/f_r3.png)

### 可变函数

PHP 支持可变函数的概念.这意味着如果一个变量名后有圆括号,PHP 将寻找与变量的值同名的函数,并且尝试执行它.可变函数可以用来实现包括回调函数,函数表在内的一些用途.

可变函数不能用于例如 echo(),print,unset(),isset(),empty(),include,require 以及类似的语言结构.需要使用自己的包装函数来将这些结构用作可变函数.

Example #1 可变函数示例
```PHP
<?php
function foo(){
    echo "In foo()<br>\n";
}

function bar($arg = ''){
    echo "In bar();argument was '$arg'.<br>\n";
}

// 使用 echo 的包装函数
function echoit($string)
{
    echo $string;
}

$func = 'foo';
$func(); // This calls foo()

$func = 'bar';
$func('test'); // This calls bar()

$func = 'echoit';
$func('test'); // This calls echoit()
?>
```
也可以用可变函数的语法来调用一个对象的方法.

Example #2 可变方法范例
```PHP
<?php
class Foo
{
    function Variable()
    {
        $name='Bar';
        $this->$name(); // This calls the Bar() method
    }

    function Bar()
    {
        echo "This is Bar";
    }
}

$foo = new Foo();
$funcname = "Variable";
$foo->$funcname(); // This calls $foo->Variable()
?>
```
当调用静态方法时,函数调用要比静态属性优先:

Example #3 Variable 方法和静态属性示例
```PHP
<?php
class Foo
{
    static $variable = 'static property';
    static function Variable()
    {
        echo 'Method Variable called';
    }
}
echo Foo::$variable; // This prints 'static property'.It does need a $variable in this scope.
$variable="Variable";
Foo::$variable(); // This calls $foo->Variable reading $variable in this scope.
?>
```

Example #4 Complex callables
```PHP
<?php
class Foo
{
    static function bar()
    {
        echo "bar\n";
    }
    function baz()
    {
        echo "baz\n";
    }
}
$func = array("Foo","bar");
$func(); // prints "bar"
$func = array(new Foo,"baz");
$func(); // prints "baz"
$func = "Foo::bar";
$func(); // prints "bar" as of PHP 7.0.0;prior,it raised a fatal error
?>
```

### 匿名函数
匿名函数(Anonymous functions),也叫闭包函数(closures),允许临时创建一个没有指定名称的函数.最经常用作回调函数(callback)参数的值.当然,也有其它应用的情况.

匿名函数目前是通过 Closure 类来实现的.

Example #1 匿名函数示例
```PHP
<?php
echo preg_replace_callback('~-([a-z])~',function ($match){
    return strtoupper($match[1]);
},'hello-world');
// 输出 helloworld
?>
```
闭包函数也可以作为变量的值来使用.PHP 会自动把此种表达式转换成内置类 Closure 的对象实例.把一个 closure 对象赋给一个变量的方式与普通变量赋值的语法是一样的,最后也要加上分号:
References:
* [Official Website](http://php.net)
* [PSR](http://www.php-fig.org/)
* [PHP100](http://www.php100.com/)

