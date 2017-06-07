---
title: PHP开发环境搭建(Ubuntu Linux)
date: 2017-05-27 16:15:43
tags:
---

本文将讲述PHP-FPM、Nginx、MySQL在Ubuntu 14.04/16.04下的安装及配置。PHP-FPM/Nginx/MySQL的关系如下图所示。

```
               ---------               -----------          ---------
浏览器-------> | Nginx | ---FastCGI--> | PHP-FPM | -------> | MySQL |
               ---------               -----------          ---------
               ------------------------------------------------------
               |                  Linux Server                      |
               ------------------------------------------------------
```

## 配置Ubuntu的软件源

因为Ubuntu默认的软件源，服务器不在国内，装起软件来会比较慢，所以得先将源改为国内的。

  * 备份源配置文件
    ```
    sudo cp /etc/apt/sources.list /etc/apt/sources.list.raw
    ```
  * 更新配置文件
    Ubuntu 14.04 使用以下命令：
    ```
    echo "
    deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
    deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
    " | sudo tee /etc/apt/sources.list
    ```

  * 使新的配置文件生效
    ```
    sudo apt-get update
    ```

## 更新系统软件

  * 更新
    ```
    sudo apt-get upgrade
    ```

  * 移除不再需要的软件包
    ```
    sudo apt-get autoremove
    ```

  * 系统重启
    ```
    sudo reboot
    ```

## 安装Nginx


```
sudo apt-get install nginx
```

## 验证Nginx安装是否成功

在浏览器中打开 http://localhost 出现：
```
Welcome to nginx!
...
```
表示安装成功了。


## 安装MySQL

```
sudo apt-get install mysql-server
```

安装的时候会询问你设置root的密码，请直接按回车，即设置密码为空。

## 验证MySQL是否安装成功

在命令行输入`mysql -uroot -hlocalhost`，出现：
```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 5.7.12-0ubuntu1.1 (Ubuntu)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```
表示安装成功！ 输入`exit 回车`，退出mysql。

## 安装PHP

```
sudo apt-get install php5 php5-cli php5-curl php5-fpm php5-intl php5-mcrypt php5-mysqlnd php5-gd
```

创建`/var/www`目录：
```
sudo mkdir /var/www
sudo chown -Rf YOUR_USER:YOU_GROUP /var/www
```

安装VIM:
```
sudo apt-get install vim
```

修改配置文件`vim /etc/nginx/sites-enabled/default`，开启PHP相关参数。将配置替换为如下：
```
server {
    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;

    root /var/www;

    index index.html index.htm;

    server_name localhost;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }
}

```

创建测试脚本：
```
echo "<?php
phpinfo();" > /var/www/phpinfo.php
```

浏览器打开：
http://localhost/phpinfo.php


安装phpMyAdmin：
进入www目录
```
wget https://files.phpmyadmin.net/phpMyAdmin/4.6.5.2/phpMyAdmin-4.6.5.2-all-languages.zip
tar -zxvf phpMyAdmin-4.6.5.2-all-languages.zip
mv phpMyAdmin-4.6.5.2-all-languages.zip phpMyAdmin
```
浏览器打开：
http://localhost/phpMyAdmin/index.php
出现页面  输入帐号root密码  进入
出现PHP相关信息，表明安装成功。

## 开发环境下，开启PHP的错误信息显示

编辑`sudo vim /etc/php5/fpm/php.ini`，找到相应配置，修改为如下：
```
error_reporting = E_ALL
display_errors = On
display_startup_errors = On
```

重启php-fpm:
```
sudo service php5-fpm restart
```


## 安装SublimeText 3（此步骤可能安装不了，可以到网上自行下载）

SublimeText 3 是目前最流行的开发编辑器。

```
sudo add-apt-repository -y ppa:webupd8team/sublime-text-3
sudo apt-get update
sudo apt-get install -y sublime-text-installer
```

## 设置SublimeText 3 等宽字体

打开菜单`Preferences -> Setting-User`，修改内容，并保存：
```
{
    "font_face": "Ubuntu Mono",
    "font_size": 12,
    "highlight_line": true,
    "translate_tabs_to_spaces": true
}
```

## 安装输入法Fcitx

因为fcitx输入法能在SublimeText 3下输入中文。

```
sudo add-apt-repository -y ppa:fcitx-team/nightly
sudo apt-get update
sudo apt-get install -y fcitx fcitx-config-gtk fcitx-sunpinyin fcitx-googlepinyin fcitx-module-cloudpinyin
sudo apt-get install -y build-essential libgtk2.0-dev
sudo apt-get install -y fcitx-table-all
sudo apt-get install im-switch
im-switch -s fcitx -z default
```

如无im-switch这个命令，则执行：
```
im-config -n fcitx
```

安装完之后，重启电脑。即可使用fcitx输入法，按Ctrl + 空格，调出输入法。

## 解决SublimeText 3 中文输入法问题

```
cd /opt/sublime_text/
sudo wget https://github.com/lyfeyaj/sublime-text-imfix/raw/master/lib/libsublime-imfix.so
```

```
sudo mv /usr/bin/subl /usr/bin/subl_raw

echo "
#!/bin/sh
export LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so
exec /opt/sublime_text/sublime_text \"\$@\""  | sudo tee /usr/bin/subl 

sudo chmod a+x /usr/bin/subl
```

编辑`vim /usr/share/applications/sublime_text.desktop`，将内容替换为：
```
[Desktop Entry]
Version=1.0
Type=Application
Name=Sublime Text
GenericName=Text Editor
Comment=Sophisticated text editor for code, markup and prose
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text %F"
Terminal=false
MimeType=text/plain;
Icon=sublime-text
Categories=TextEditor;Development;
StartupNotify=true
Actions=Window;Document;

[Desktop Action Window]
Name=New Window
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text -n"
OnlyShowIn=Unity;

[Desktop Action Document]
Name=New File
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text --command new_file"
OnlyShowIn=Unity;
```
