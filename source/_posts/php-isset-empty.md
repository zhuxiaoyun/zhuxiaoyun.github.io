---
title: PHP语言-isset & empty
date: 2017-07-07 09:04:20
tags:
---

php的坑
程序中经常会使用isset，empty判断数组中的某个key是否存在，有值。数组使用这两个api肯定没有问题的，但是如果是string的话会发生什么事情呢？
```php
$expected_array_got_string = 'somestring';
var_dump(isset($expected_array_got_string['some_key']));
```
上面的代码结果是true，还是false？下面呢？
```php
$expected_array_got_string = 'somestring';
var_dump(empty($expected_array_got_string['some_key']));
```
如果不是看了官方的文档，打死我我也猜不到。让我们看看官方怎么说的.
先是[isset](http://php.net/manual/zh/function.isset.php)

PHP 5.4 changes how isset() behaves when passed string offsets.

```php
$expected_array_got_string = 'somestring';
var_dump(isset($expected_array_got_string['some_key']));
var_dump(isset($expected_array_got_string[0]));
var_dump(isset($expected_array_got_string['0']));
var_dump(isset($expected_array_got_string[0.5]));
var_dump(isset($expected_array_got_string['0.5']));
var_dump(isset($expected_array_got_string['0 Mostel']));
```
以上例程在PHP 5.3中的输出：
```
bool(true)
bool(true)
bool(true)
bool(true)
bool(true)
bool(true)
```
以上例程在PHP 5.4中的输出：
```
bool(false)
bool(true)
bool(true)
bool(true)
bool(false)
bool(false)
```
[empty的表现](http://php.net/manual/zh/function.empty.php)

PHP 5.4 修改了当传入的是字符串偏移量时， empty() 的行为

```php
$expected_array_got_string = 'somestring';
var_dump(empty($expected_array_got_string['some_key']));
var_dump(empty($expected_array_got_string[0]));
var_dump(empty($expected_array_got_string['0']));
var_dump(empty($expected_array_got_string[0.5]));
var_dump(empty($expected_array_got_string['0.5']));
var_dump(empty($expected_array_got_string['0 Mostel']));
```
以上例程在PHP 5.3中的输出：
```
bool(false)
bool(false)
bool(false)
bool(false)
bool(false)
bool(false)
```
以上例程在PHP 5.4中的输出：
```
bool(true)
bool(false)
bool(false)
bool(false)
bool(true)
bool(true)
```
是不是很惊讶。直到我看到官方的这个文档我才相信问题真的出现在isset这个api上（哭）。
为了让大家都知道有这个问题，特定把这个分享出来，大家在用这两个api的时候，需要认真考虑一下。
这也让我想到一个问题，我们是不是应该提供一些工具类(系统也确实有很多Toolkit)，来封装类似这样的api（否则哪天我们真的升级php版本的话那会不会是一个噩梦）。比如：
```php
class XxxxToolkit
{
    public static function  isset($value)
    {
       if(is_array($value)){xxxxx}else{xxxxx}
    }
}

class Client
{
 $var = "xxxxx";
 XxxxToolkit::isset($var);
}
```
欢迎大家讨论。


