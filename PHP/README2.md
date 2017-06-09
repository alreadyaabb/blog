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
References:

* [Official Website](http://php.net)
* [PSR](http://www.php-fig.org/)
* [PHP100](http://www.php100.com/)

