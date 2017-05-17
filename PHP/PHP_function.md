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
