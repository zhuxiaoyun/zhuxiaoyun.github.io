---
title: Linux查询
date: 2018-08-13 15:31:55
categories:
- 实用技术 # 一级分类
- 个人博客 # 二级分类 
tags:
- 实用
- 个人博客
---

### 查看当前路径

pwd

### 查看当前用户

who

### ll列出当前目录下 以la开头的文件

1、直接使用通配符
ll la*
2、使用管道，将ls的输出送入grep这个程序来实现
ls -1 | grep "^la"
-1选项表示将列出的所有文件排成一列，方便grep的匹配（grep按行匹配）。
ls -a | grep "la\*"

### 查找文件

find /etc -name "la\*"
查找/etc目录下，以"la"开头命名的文件，/ 表示全局

### grep 查看匹配日志的前后几行

如果在只是想匹配模式的上下几行，grep可以实现。
 
$grep -5 'parttern' inputfile //打印匹配行的前后5行
 
$grep -C 5 'parttern' inputfile //打印匹配行的前后5行
 
$grep -A 5 'parttern' inputfile //打印匹配行的后5行
 
$grep -B 5 'parttern' inputfile //打印匹配行的前5行

### grep 多次搜索关键字



### 查看某一个文件第5行到第10行

 sed -n '5,10p' filename 这样你就可以只查看文件的第5行到第10行。
