---
title: PHP获取时间的坑
date: 2017-05-27 14:25:31
tags:
---
php的 [date()](http://www.php.net/manual/zh/function.date.php) 函数，string date ( string $format [, int $timestamp ] )，可以格式化一个
本地时间或日期，返回将整数timestamp按照给定字串而产生的字串，如果没有给定时间戳，则使用本地当前时间，即timestamp是可选的，默认值是time()。
date()的使用非常灵活，下面介绍使用date()获取当月的第一天、当月的最后一天，下月的第一天、下月的最后一天
### 获取当前时间
``` bash
echo date('Y-m-d');
echo date('Y-m-d H:i:s');
```
### 获取当前月的第一天
``` bash
echo date('Y-m-01');
```
### 获取当前月的最后一天
``` bash
echo date('Y-m-t');
```
### 获取下一月的最后一天
``` bash
echo date('Y-m-t', strtotime('+1 month'));
```
### 获取下一月的第一天
``` bash
echo date('Y-m-01',  strtotime('+1 month'));
```
Warning：请注意，这里有一个坑，当当前月为大月时，当天的时间为最后一天，如2017-05-31，加一个月取第一天并不会得到下一个月的第一天，而会顺延到下下月的某一天（取决于多的天数），所以正确的应该是，当前月第一天加一个月
以下是正确的写法：
### 获取下一月的第一天
``` bash
echo date('Y-m-01', strtotime('+1 month', strtotime(date('Y-m-01'))))."<br>";
```
### 获取下一月的最后一天
``` bash
echo date('Y-m-t', strtotime('+1 month', strtotime(date('Y-m-01'))))."<br>";
```

同样，php的Symfony框架提供的[date_modify()](https://twig.sensiolabs.org/doc/2.x/filters/date_modify.html)，twig扩展也有同样的问题，给遇到坑的小伙伴们助攻！