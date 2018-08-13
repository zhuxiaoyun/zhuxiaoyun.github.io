---
title: Linux基础
date: 2017-09-26 14:17:11
categories:
- 实用技术 # 一级分类
- 个人博客 # 二级分类 
tags:
- 实用
- 个人博客
---
## Linux简介

## 基本命令

man cd ls cp rm mkdir rmdir chmod chown sudo ssh scp top cat kill
详细使用请参见[Linux常用命令的详细用法](http://blog.csdn.net/ljianhui/article/details/11100625)，自行实践。
在线查询Linux命令的用法请参见[Linux 手册](http://man.linuxde.net/)
```
常见命令用法 
cd /var/www    切换到 /var/www目录下
ls             列出所在目录下的内容 (ll 更详细)
cp -r /var/www/edusoho /data/www    将 /var/www/eudosho 拷贝到 /data/www 目录下
mkdir abc      当前目录下, 新建 abc文件夹
rm -rf abc     强制删除abc, 如果abc是文件加, 也可用命令 rmdir abc
chown -r user:group abc   将abc文件或目录的用户组和用户改为 group 和 user
chmod -Rf 777 abc  给abc文件授权, 所有人都能读写
sudo rm -rf abc  使用系统管理员的权限进行操作
ssh -p1122 root@111.22.22.11   远程访问 111.22.22.11 服务器, 一般端口为 1122 或 22, 会提示输入密码
scp -r -P 1122 allSqls.sql root@111.22.22.11:~  将allSqls.sql拷贝到远程服务器的 ~ 目录下, 会提示输入密码
top  查看服务器状态和进程
tail -f a.txt  查看a.txt最后几行, 会实时刷新
sudo vim a.txt 查看文本, 能进行编辑, 输入 小写字母i, 可进行编辑, 编辑完后, 按 esc, 冒号, wq, 回车, 进行保存
cat a.txt  显示所有内容
lsof -i:80  查看使用80端口的所有进程
kill -9 1202  删除进程号为1202的进程, 如lsof -i:80命令列出来的 PID
```

## 管道（pipe）

  管道的操作命令符是“|”，它负责将前一条指令传出的正确输出信息（即standard output, 不包括standard error）作为标准输入（standard input）传递个下一个命令。
  比如下述命令cat输出的文件内容传递给grep命令，按照限定的字符串进行筛选，结果输出到standard output.
  ```
  cat error.log | grep 'RuntimeException' 
  ```
详情参见：[管道和过滤器](http://c.biancheng.net/cpp/html/2732.html)
进一步了解参见：[管道机制](http://oilbeater.com/linux/2012/05/29/linux_pipe.html)

## 输入输出重定向
  
  * 标准输入重定向，使用 < 或 << , 从指定文件获取输入信息，如下命令将从message.log中查询含有"locale"的行并输出到屏幕：
  ```
    grep "locale" < message.log
  ```

  * 标准输出重定向（stdout），使用 > 或 >> 表达，比如下面，执行test.php，将结果输出到result.html；
  ```
    php test.php > result.html
  ```

 详细说明和使用参见：[Linux Shell 输入输出重定向详细分析](http://www.cnblogs.com/chengmo/archive/2010/10/20/1855805.html)

## Linux用户、用户组、文件权限
Linux是一个完整的多用户多任务操作系统，允许多个用户在同一时间登录同一系统执行各自不同的任务。
Linux用户分成三种：  

- 超级用户，拥有对系统的最高管理权限，默认是root；
- 普通用户，只能对自己目录下的文件进行访问和修改，具有登录系统的权限，比如ftp、www等；
- 虚拟用户／伪用户，不能登录系统，它们主要是为方便管理系统，满足相应系统进程对文件属主的要求，比如内置的bin、adm、nobody等
将用户分组是Linux系统对用户进行管理和控制访问权限的手段，很大程度上简化了管理工作。

用户及文件详细操作命令参见[Linux用户和用户组管理](http://www.runoob.com/linux/linux-user-manage.html)。

## 编辑器VIM
vim，神一样的编辑器！
vim = vi improved
详细命令操作参见拓展阅读[简明Vim练级攻略－酷壳](http://coolshell.cn/articles/5426.html)

!!! note "注意"
    已使用过VIM的同学，请学习到文章中的第三级。未使用过的同学，请学习到文章中的第一级。剩余部分列入你的当月学习的Todo List。


## Ubuntu的软件的安装
apt-get是ubuntu、debian的包管理工具，与red hat的yum类似，一般需要root权限执行，所以通常是 sudo apt-get ...。
```
查找：
apt-cache search querystring 

安装：
apt-get install packagename

卸载：(保留配置文件)
apt-get remove packagename

完全卸载：（不保留配置文件）
apt-get --purge remove packagename
```
详细操作参见[apt-get 详细命令](http://www.howtogeek.com/63997/how-to-install-programs-in-ubuntu-in-the-command-line/)

## 课后练习

```
1．  登录进入系统。
2．  使用简单命令：date，cal，who，echo，clear等，了解Linux命令格式。
3．  浏览文件系统：
(1)    运行pwd命令，确定当前工作目录。
(2)    运行ls –l命令，理解各字段含义。
(3)    使用cd命令，将工作目录改到（/）上。
运行ls –l命令，结合文件系统的知识，了解各目录的作用。
(5)    直接使用cd命令，或到哪里了？用pwd验证。
(6)    用mkdir在根目录下建立一个子目录subdir。在用户主目录下建立可以吗？
(7)    将工作目录改到subdir下。
4．  文件操作：
(1)    验证当前工作目录在subdir。
(2)    运行date > file1，然后运行cat file1，看到什么信息？
(3)    运行cat subdir，会有什么结果？为什么？
(4)    利用man命令显示date命令的使用说明。
(5)    运行man date >> file1，看到什么？ 运行cat file1，看到什么？
(6)    用vim 建立用户目录下的ex文件，输入一些内容。
(8)    显示file1的前十行，后十行。
(9)    运行cp file1 file2，然后ls –l，看到什么？ 运行mv file2 file3，然后ls –l，看到什么？ 运行cat f*，结果怎样？
(10)   运行rm file3，然后ls –l，结果如何？
(11)   更改file1的文件所属的用户组为root。
```

## 拓展阅读

* [Linux Users and Groups(中文版)](https://wiki.archlinux.org/index.php/Users_and_groups_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
* [一些常用的linux命令-gitbook](https://wujin.gitbooks.io/-linux-wu/content/)
* [vim adventures - Learning VIM while playing a game](http://vim-adventures.com/)

