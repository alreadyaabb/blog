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
