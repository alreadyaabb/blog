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
