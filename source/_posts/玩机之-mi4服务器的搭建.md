---
title: 玩机之-mi4服务器的搭建
date: 2018-06-15 15:27:02
tags: [mi4, 服务器, deploy,ksweb, Termux, apache, php, nginx]
category: 服务器
---
### 一，使用ksweb
#### 1.KSWEB介绍
ksweb 是一款收费软件，在谷歌应用商店可以下载，他是一个集成appache,php,lighttpd,nginx,mysql,ftp,和其他一些方便的配置工具，且无须root就可以开启服务器，可以说是十分强大了
我的小米四是armV7，32位操作系统
在我的小米四miui系统上开启nginx似乎不太行，所以我只开启了lighttpd服务器，软件自动配置好最新的php版本，十分人性化


#### 2.遇到的问题
某些系统在使用ksweb只有在亮屏幕的情况下使用，所以遇到这种问题，刷机可以解决...笔者小米4刷了个第三方包Aosp就没问题了，就是耗电有点厉害

 

### 二，使用linux-deploy
#### 1.介绍deploy
对于搞基爱好者来说，发现安卓手机可以运行Linux真是像发现新大陆一样（好吧村通网系列），Linux-deploy的安装百度有很多，这里就不多介绍了
你可以安装ubuntu18,CentOS,archlinux,kali...等等，并且可以安装图形界面，还有声卡等支持

#### 2.搭建nginx, php
这里我安装了archlinux，至于如何安装，网上教程很多，这里就不多费口舌了,下面介绍如何配置php,nginx
##### 安装nginx php php-fpm
`pacman -S vim`
`pacman -S nginx`
`pacman -S php`
`pacman -S php-fpm`
##### 修改nginx配置文件 nginx.conf
`sudo vim /etc/nginx/nginx.conf`
去掉注释，如图
![nginx.conf](/blog/myimg/server/nginx.conf.png)
改变监听端口
![nginx.conf-server](/blog/myimg/server/nginx.conf-server.png)
网站根目录
![nginx-location](/blog/myimg/server/location.png)

##### 修改php配置文件 www.conf
`sudo vim /etc/php/php-fpm.d/www.conf`
定位`listen`
修改为 `listen 127.0.0.1:9002`
这里的端口可以自选，但是要和nginx.conf对应
![www.conf](/blog/myimg/server/www.conf.png)


#### 3.启动nginx,php-fpm
`php-fpm`
`nginxx`
查看启动情况
`netstat -lant`
![netstat -lant](/blog/myimg/server/netstat-lant.png)
  

如果`command not found`
则需要安装`net-tools`
输入
`pacman -S net-tools`

手机端访问127.0.0.1:8050

    welcome nginx

#### 4.在网站根目录下新建phpinfo.php
```shell
cd /user/share/nginx/html/
vim phpinfo.php
```
内容如下
```php
    <?php
        phpinfo();
    ?>
```
手机端访问
```
127.0.0.1:8050/phpinfo.php
```

#### 6.遇到的问题

##### permisson deny，无法手机端访问php文件
![nginx-error-php](/blog/myimg/server/nginx-error-php.png)
![nginx-error-php1](/blog/myimg/server/nginx-error-php1.png)
  
~~笔者已经抛弃用Linux-deploy搭建服务器了，原因是nginx配置php的时候总是出现各种Permission deny问题，然而似乎无法解决（我不会而已。。）~~
##### 转换思路，直接用apache2 和libapache2-mod-php libapache2-mod-php7.2 模块
先找一下包的名字
```shell
apt list | grep php
apt list | grep apache
```
安装
```shell
apt-get install apache2 libapache2-mod-php libapache2-mod-php7.2
```
配置路径在`/etc/apache2/sites-available/000-default.conf`
启动
```shell
apachectl -M  //看看是否加载了 php7_module 
apachectl start //启动
netstat -lntp //看看启动没有,httpd 
```

### 三，termux
#### 1.介绍termux
Termux就更强大了,可以在F-droid，Google Play上面下载
Termux官方主页 **https://termux.com/**
github **https://github.com/termux/**
下面介绍四步搭建php+nginx
#### 第一，安装相应的包
`pkg install nginx php-fpm php`
#### 第二，配置nginx.conf
`termux-chroot` //会发现绝对路径变了
如果显示Permission deny，则安装proot
`pkg install proot` 
![termux-chroot1.png](/blog/myimg/server/termux/termux-chroot1.png)
`vim /etc/nginx/nginx.conf`
修改三个地方，分别是监听端口，加载php，网站根目录
![nginx.conf](/blog/myimg/server/termux/nginx.conf.png)

#### 第三，配置www.conf
`vim /etc/php-fpm.d/www.conf`
定位listen
输入`/^listen` 回车
修改为
`listen = 127.0.0.1:9000`
![termux-www.conf.png](/blog/myimg/server/termux/www.conf.png)

#### 第四，开启php-fpm,nginx
输入
`php-fpm`
`nginx`
`手机端访问127.0.0.1:8050`

#### 第五，测试php
到这里php，nginx就配置完了，接下来在网站根目录新建立一个php文件
`cd /usr/share/nginx/html/`
`vim phpinfo.php`
内容如下

```php
<?php
phpinfo();
?>
```

手机端访问 
```
127.0.0.1:8050/phpinfo.php
```
![phpinfo](/blog/myimg/server/termux/phpinfo.png)  



下面介绍Termux 如何搭建apache2 和 php-apache
# termux apache php的配置

https:/.csdn.net/aoli_shuai/article/details/78847700

```shell
pkg install apache2  php-apache
termux-chroot
vim  /usr/etc/apache2/httpd.conf
```

找到
```conf
AddType application/x-compress .Z
AddType application/x-gzip .gz .tgz
```

增加

```conf
AddType application/x-httpd-php .php
```


找到

```conf
<IfModule dir_module> DirectoryIndex index.html </IfModule> 
```

增加为

```conf
<IfModule dir_module> DirectoryIndex index.html index.php </IfModule> 
```



 检查有没有加载PHP7的模块
 ```shell
termux-chroot
ls /usr/libexec/apache2/  | grep php
```

查看配置文件httpd.conf中有没有加载libphp7.so的配置


如果没有，则添加一下内容，注意模块的名字
```
LoadModule php7_module libexec/apache2/libphp7.so
```

检测配置文件语法是否有错误
```
[root@shuai-01 ~]# /usr/local/apache2.4/bin/apachectl -t Syntax OK 
```
重新加载配置文件
```
[root@shuai-01 ~]# /usr/local/apache2.4/bin/apachectl graceful 
```
配置成功


有关更多的Termux玩法请看
https://wiki.termux.com/wiki/Main_Page
http://www.sqlsec.com/2018/05/termux.html



### 四，结语
手机做服务器玩玩就好，目前还在探索中...