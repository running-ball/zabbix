# zabbix
zabbix grafana安装部署


一、安装nginx：
安装依赖包：
yum -y install gcc gcc-c++ autoconf automake zlib zlib-devel openssl openssl-devel pcre* make gd-devel libjpeg-devel libpng-devel libxml2-devel bzip2-devel libcurl-devel libevent-devel

创建用户：
useradd nginx -s /sbin/nologin -M

下载nginx软件包并进入到目录中：
wget http://nginx.org/download/nginx-1.12.0.tar.gz && tar xvf nginx-1.12.0.tar.gz && cd nginx-1.12.0
 编译：
./configure --prefix=/usr/local/nginx-1.12.0 --user=www --group=www --with-http_ssl_module --with-http_v2_module --with-http_stub_status_module --with-pcre

make && make install
ln -s /usr/local/nginx-1.12.0 /usr/local/nginx    ==>创建软链接
参数解释：
--with-http_stub_status_module：支持nginx状态查询 --with-http_ssl_module：支持https --with-http_spdy_module：支持google的spdy,想了解请百度spdy,这个必须有ssl的支持 --with-pcre：为了支持rewrite重写功能，必须制定pcre
 
二、安装PHP
下载PHP安装包：
wget http://cn2.php.net/get/php-7.1.8.tar.gz/from/this/mirror
解压并编译：
mv mirror php-7.1.8.tar.gz && tar xvf php-7.1.8.tar.gz && cd php-7.1.8

./configure --prefix=/usr/local/php-7.1.8 --with-config-file-path=/usr/local/php-7.1.8/etc --with-bz2 --with-curl --enable-ftp --enable-sockets --disable-ipv6 --with-gd --with-jpeg-dir=/usr/local --with-png-dir=/usr/local --with-freetype-dir=/usr/local --enable-gd-native-ttf --with-iconv-dir=/usr/local --enable-mbstring --enable-calendar --with-gettext --with-libxml-dir=/usr/local --with-zlib --with-pdo-mysql=mysqlnd --with-mysqli=mysqlnd --with-mysql=mysqlnd --enable-dom --enable-xml --enable-fpm --with-libdir=lib64 --enable-bcmath

make && make install
ln -s /usr/local/php-7.1.8 /usr/local/php

cp php.ini-production /usr/local/php/etc/php.ini
cd /usr/local/php/etc/

cp php-fpm.conf.default php-fpm.conf
 
修改php.ini参数：（zabbix环境需要修改的参数）
max_execution_time = 300  
memory_limit = 128M
post_max_size = 16M
upload_max_filesize = 2M
max_input_time = 300  
date.timezone = Asia/Shanghai


三、安装MySQL
下载MySQL安装包：
wget http://dev.mysql.com/get/Downloads/MySQL-5.5/mysql-5.5.49.tar.gz(这个页面不能用了)
Wget http://mirrors.sohu.com/mysql/MySQL-5.7/mysql-5.7.14-linux-glibc2.5-x86_64.tar.gz
