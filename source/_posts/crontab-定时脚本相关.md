---
title: crontab 定时脚本相关
date: 2018-08-13 15:01:50
categories:
- 实用技术 # 一级分类
- 个人博客 # 二级分类 
tags:
- 实用
- 个人博客
---

### 常用命令
    crontab -l  查看生效的定时任务列表
    crontab -l -u XXX 列出XXX用户的所有定时任务
    crontab -e  编辑定时任务

### 基本格式
\*  \*  \*  \*  \*　　command 
分  时   日  月   周　 命令 
第1列表示分钟1～59 每分钟用\*或者 \*/1表示 
第2列表示小时1～23（0表示0点） 
第3列表示日期1～31 
第4列表示月份1～12 
第5列标识号星期0～6（0表示星期天） 
第6列要运行的命令

#### 例子
30 21 \* \* \* /usr/local/etc/rc.d/lighttpd restart 每晚的21:30重启apache。 
45 4 1,10,22 \* \* /usr/local/etc/rc.d/lighttpd restart 每月1、10、22日的4 : 45重启apache。 
10 1 \* \* 6,0 /usr/local/etc/rc.d/lighttpd restart 每周六、周日的1 : 10重启apache。 

### 定时任务不执行，排查步骤

#### 检查日志

1、查看是否有设置输出日志，如：
```
01 02 * * * /var/www/lms/app/console statistics:testpaper-result && date >> ~/crontab.log
```
2、没有输出日志，检查执行日志
    /var/log/crontablog

有执行日志，很明显，任务计划确实在正常执行着，看来问题在脚本上了。

#### 检查脚本

1、直接执行
检查脚本第一步，直接按照 crontab 里面的命令行，执行脚本：
直接执行成功，而放到 crontab 就失败，经验告诉我肯定的脚本环境变量有问题了！

2、环境变量
于是在脚本里面载入环境变量：

```

#!/bin/bash
#先载入环境变量
source /etc/profile
#其他代码不变

```
结果还是不行

3、系统邮件
经验告诉我，crontab 执行失败，如果没有屏蔽错误的话，会产生一个系统邮件，

位置在 /var/spool/mail/root


cat /var/spool/mail/root 发现是否有报错

#### 分析总结

Linux 系统里面计划任务，crontab 没有如期执行这是运维工作中比较常见的一种故障了，根据经验，大家可以从如下角度分析解决:

①、检查 crontab 服务是否正常
这个一般通过查看日志来检查，也就是前文提到的 /var/log/cron 或 /var/log/messages，如果里面没有发现执行记录，那么可以重启下这个服务：
```
1.在系统中有service这个命令时：
service crond stop | start | restart
2.linux发行版本没有service这个命令时：
/etc/init.d/cron stop | start | restart
```

②、检查脚本的执行权限
一般来说，在 crontab 中建议使用 sh 或 bash 来执行 shell 脚本，避免因脚本文件的执行权限丢失导致任务失败。当然，最直接检查就是人工直接复制 crontab -l 里面的命令行测试结果。

③、检查脚本需要用到的变量
和上文一样，通常来说从 crontab 里面执行的脚本和人工执行的环境变量是不一样的，所以对于一些系统变量，建议写绝对路径，或使用 witch 动态获取，比如  sudo_bin=$(which sudo) 就能拿到 sudo 在当前系统的绝对路径了。

④、放大招：查看日志
其实，最直接最有效的就是查看执行日志了，结合 crontab 执行记录，以及 crontab 执行出错后的系统邮件，一般都能彻底找到失败的原因了！当然，要记住在 crontab 中如果屏蔽了错误信息，就不会发邮件了。

这又让我想起了如果 crontab 未屏蔽日志，可能会导致硬盘 inode 爆满 ==> 历史文章传送门 ，感兴趣的童鞋也可以谷歌一下 /var/spool/clientmqueue/ 这个关键词了解下。

#### 警告
千万别乱运行crontab -r。它从Crontab目录（/var/spool/cron）中删除用户的Crontab文件。删除了该用户的所有crontab都没了。
