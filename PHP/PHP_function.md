# PHP_function

## range()
```PHP
range(min,max,step);
```
range() 函数创建一个包含制定范围的元素的数组。该函数返回一个包含从 min 到 high 之间的元素的数组。

|参数|描述|
|---|----|
|min|必需。规定数组的最低值|
|max|必需。规定数组的最高值|
|step|可选。规定元素之间的步进制。默认为1|

**Note**:如果 min 参数大于 max 参数，则创建的数组将是从 max 到 min。

Example #1
```PHP
<?php
$number = range(0,50,10);
print_r ($number);
?>
```
![range1](https://github.com/alreadyaabb/blog/blob/append_php_info/images/range_ex1.png)

Example #2
```PHP
<?php
$letter = range("a","d");
print_r ($letter);
?>
```
![range2](https://github.com/alreadyaabb/blog/blob/append_php_info/images/range_ex2.png)

## array_map()
```PHP
array_map(myfunction,array1,array2,array3...)
```
array_map() 函数将用户自定义函数作用到数组的每个值上，并返回用户自定义函数作用后的带有新值的数组。

|参数|描述|
|---|---|
|myfunction|必需。用户自定义函数的名称，或者是 null。|
|array1|必需。规定数组。|
|array2|可选。规定数组。|
|array3|可选。规定数组。|

**Note**:回调函数接受的参数数目应该和传递给 array_map() 函数的数组数目一致。

**提示**:您可以向函数输入一个或者多个数组。

Example #1
```PHP
<?php
function myfunction($v)
{
    if ($v === "Dog")
    {
        return "Fido";
    }
    return $v;
}

$a = array("Horse","Dog","Cat");
print_r(array_map("myfunction",$a));
?>
```
![array_map1](https://github.com/alreadyaabb/blog/blob/append_php_info/images/array_map1.png)

Example #2
```PHP
<?php
function myfunction($v1,$v2)
{
    if($v1===$v2)
    {
        return "same";
    }
    return "different";
}
$a1=array("Horse","Dog","Cat");
$a2=array("Cow","Dog","Rat");
print_r(array_map("myfunction",$a1,$a2));
?>
```
![array_map2](https://github.com/alreadyaabb/blog/blob/append_php_info/images/array_map2.png)

Example #3
```PHP
<?php
function myfunction($v)
{
    $v=strtoupper($v);
    return $v;
}
$a=array("Animal"=>"horse","Type"=>"mammal");
print_r(array_map("myfunction",$a));
?>
```
![array_map3](https://github.com/alreadyaabb/blog/blob/append_php_info/images/array_map3.png)

Example #4
```PHP
<?php
$a1=array("Dog","Cat");
$a2=array("Puppy","Kitten");
print_r(array_map(null,$a1,$a2));
?>
```
![array_map](https://github.com/alreadyaabb/blog/blob/append_php_info/images/array_map4.png)

## 字符串大小写处理函数
### PHP strtoupper() 函数
把所有字符转换为大写：
```PHP
strtoupper(string)
```
**注释**:该函数是二进制安全的。

|参数|描述|
|---|---|
|string|必需。规定要转换的字符串。|

### PHP strtolower() 函数
把字符串转换为小写：
```PHP
strtolower(string)
```
**注释**:该函数是二进制安全的。

|参数|描述|
|---|---|
|string|必需。规定要转换为的字符串。|

### PHP lcfirst() 函数
把字符串中的首字符转换为小写：
```PHP
lcfirst(string)
```

|参数|描述|
|---|---|
|string|必需。规定要转换的字符串。|

### PHP unfirst() 函数
把字符串中的首字符转换为大写：
```PHP
ucfirst(string)
```

|参数|描述|
|--|--|
|string|必需。规定要转换的字符串。|

### PHP ucwords() 函数
把字符串中每个单词的首字符转换为大写：
```PHP
ucwords(string)
```

|参数|描述|
|--|--|
|string|必需。规定要转换的字符串。|

## implode()
```PHP
implode(separator,array)
```
implode() 函数返回由数组元素组合成的字符串。
**注释**:implode() 函数接受两种参数顺序。但是由于历史原因，explode() 是不行的，必须保证 separator 参数在 string 参数之前才行。
**注释**:implode() 函数的 separator 参数是可选的。但是为了向后兼容，推荐您使用两个参数。
**注释**:该函数是二进制安全的。

|参数|描述|
|---|---|
|separator|可选。规定数组元素之间放置的内容。默认是""(空字符串)。|
|array|必需。要组合为字符串的数组。|

Example #1
```PHP
<?php
$arr = array('Hello','World!','I','love','Shanghai!');
echo implode(" ",$arr)."<br>";
echo implode("+",$arr)."<br>";
echo implode("-",$arr)."<br>";
echo implode("X",$arr);
?>
```
![implode](https://github.com/alreadyaabb/blog/blob/append_php_info/images/implode1.png)
## settype()
```PHP
bool settype(mixed &$var , string $type)
```
settype() 函数将变量 var 的类型设置成 type.

|参数|描述|
|---|---|
|var|要转换的变量.|
|type|type 的可能值为:boolean,integer,float,string,array,object,null|

返回值:成功时返回 true,或者失败时返回 false.

Example #1 settype() 示例
```PHP
<?php
$foo = "5bar"; //string
$bar = true; // boolean
settype($foo, "integer"); // $foo 现在是 5 (integer)
settype($bar, "string"); // $bar 现在是 "1" (string)
?>
```

## var_dump()
```PHP
void var_dump (mixed $expressing [,mixed $...])
```
此函数显示关于一个或多个表达式的结构信息,包括表达式的类型与值.数组将递归展开值,通过缩进显示其结构.
**Tip** 和直接将结果输出到浏览器一样,可使用输出控制函数来捕获当前函数的输出,然后(例如)保存到一个 string 中.

|参数|描述|
|---|---|
|expression|你要打印的变量.|

Example #1 var_dump() 例子
```PHP
<?php
$a = array(1,2,array("a","b","c"));
var_dump($a);
?>
```

输出结果:

![var_dump1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/var_dump1.png)

```PHP
<?php
$b = 3.1;
$c = true;
var_dump($b,$c);
?>
```

输出结果:

![var_dump2.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/var_dump2.png)

## isset()
```PHP
bool isset ( mixed $var [, mixed $... ] )
```
isset() 函数用于检验是否设置,并且不是 NULL.

|参数|描述|
|---|---|
|var|要检查的变量.|
|...|其他变量.|

返回值:如果 var 存在并且值不是 NULL 则返回 TRUE,否则返回 FALSE.

**Warning**:isset() 只能用于变量,因为传递任何其他参数都将造成解析错误.若想检测常亮是否已设置,可使用 defined() 函数.

**Note**:因为 isset() 是一个语言构造器而不是一个函数,不能被可变函数调用.

**Note**:如果使用 isset() 来检查对象无法访问的属性,如果  _\_isset() 方法已经定义则会调用这个重载方法.
Example #1 isset() 例子
```PHP
<?php

$var = '';

// 结果为 TRUE,所以后边的文本将被打印出来.
if(isset$var()){
    echo "This var is set so I will print.";
}

// 后边的例子中,我们将使用 var_dump 输出 isset() 的返回值.
// the return value of isset().
$a = "test";
$b = "anothertest";

var_dump(isset($a)); // TRUE
var_dump(isset($a,$b)); //TRUE

unset($a);

var_dump(isset($a)); //FALSE
var_dump(isset($a,$b)); //FALSE

$foo = NULL;
var_dump(isset($foo)); // FALSE

?>
```
![isset1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/isset1.png)

## import_request_variables()
```PHP
bool import_request_variables(string $types [,string $prefix])
```
将 GET/POST/Cookie 变量导入到全局作用域中.如果你禁止了 register_globals,但又想用到一些全局变量,那么此函数就很有用.
你可以使用 type 参数指定需要导入的变量.可以用字母'G','P'和'C'分别表示 GET,POST 和 Cookie.这些字母不区分大小写,所以你可以使用'g','p'和'c'的任何组合.POST 包含了通过 POST 方法上传的文件信息.注意这些字母的顺序,当使用'gp'时,POST 变量将使用相同的名字覆盖 GET 变量.任何 GPC 以外的字母都将被忽略.

prefix 参数作为变量名的前缀,置于所有被导入到全局作用域的变量之前.所以如果你有个名为"userid"的 GET 变量,同时提供了"pref_"作为前缀,那么你将获得一个名为 $pref_userid 的全局变量.
如果你对导入其他全局变量(例如 SERVER 变量)感兴趣,请考虑使用 extract().
**Note**:虽然 prefix 参数是可选的,但如果不指定前缀,或者指定一个空字符串作为前缀,你将获得一个 E_NOTICE 级别的错误.使用默认错误报告级别是不显示注意(Notice)级别的错误的.
```PHP
<?php
// 此处将导入 GET 和 POST 变量
// 使用"rvar_"作为前缀
import_request_variables("gP","rvar");
echo $rvar_foo;
?>
```
## define()
```PHP
bool define(string $name,mixed $value[,bool $case_insensitive = false])
```
在运行时定义一个常量.

|参数|说明|
|:--:|----|
|name|常量名.|
|value|常量的值;在 PHP 5 中,value 必须是标量(integer,float,string,boolean,NULL)在 PHP 7 中还允许是个 array 的值.|
|case_insensitive|如果设置为 TRUE ,该常量则大小写不敏感.默认是大小写敏感的.比如,CONSTANT 和 Constant 代表了不同的值.|

返回值:成功时返回 TRUE,或者在失败时返回 FALSE.

**Warning** 常量还可以定义为 resource 类型,但并不推荐这样做,因为可能会有不可预知的行为发生.

**Note**:大小写不敏感的常量以小写的方式存储.

Example #1 定义常量
```PHP
<?php
define("CONSTANT","Hello world.");
echo CONSTANT; // 输出 "Hello world."
echo Constant; // 输出 "Constant"并导致 Notice

define("GREETING","Hello you.",true);
echo GREETING; // 输出 "Hello you."
echo Greeting; // 输出 "Hello you."

// PHP 7 起就可以运行了
define('ANIMALS',array(
    'dog',
    'cat',
    'bird'
));
echo ANIMALS[1]; // 输出 "cat"

?>
```
![define1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/define1.png)

## defined()
```PHP
bool defined(string $name)
```
检查该名称的常量是否已定义.

|参数|说明|
|:--:|----|
|name|常量的名称.|

返回值:如果名称 name 的常量已定义,返回 TRUE;未定义则返回 FALSE.
**Note**:如果你要检查一个变量是否存在,请使用 isset().defined() 函数仅对 constants 有效.如果你要检测某个函数是否存在,使用 function_exists().

Example #1 检查常量
```PHP
<?php
/* Note the use of quotes, this is important. This example is checking
 * if the string 'TEST' is the name of a constant named TEST */
 if(defined('TEST')){
     echo TEST;
 }
 ?>
```

## get_defined_constants()
```PHP
array get_defined_constants([bool $categorize = false])
```
返回当前所有已定义的常量名和值.这包含 define() 函数所创建的,也包含了所有扩展所创建的.

|参数|说明|
|:--:|----|
|categorize|让此函数返回一个多维数组,分类为第一维的键名,常量和它们的值位于第二维.|

返回值:返回的数组为 常量名=>常量值,也可以按注册变量的扩展名称来分组.

Example #0
```PHP
<?php
define("MY_CONSTANT",1);
print_r(get_defined_constants(true));
?>
```
由于结果过于长，笔者只节选了部分结果，以上例程的输出类似于:
![get_defined_constants1.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/get_defined_constants1.png)

Example #1 get_defined_constants() 例子
```PHP
<?php
print_r(get_defined_constants());
?>
```
以上例程的输出类似于:
![get_defined_constants2.png](https://github.com/alreadyaabb/blog/blob/append_php_info/images/get_defined_constants2.png)
