# My-LNMP

个人lnmp配置

## 软件版本

Linux: Ubuntu 14.04
PHP: 7.0.1
Nginx: 1.9.9
MySQL: 5.7.10

## 安装Nginx

依赖关系:
```
apt-get install build-essential libtool libpcre3 libpcre3-dev openssl libssl-dev
```

配置:
./configure --user=www-data --group=www-data --with-http_stub_status_module --with-http_ssl_module --with-file-aio --with-http_realip_module  

安装:
```
make && make install
```

Nginx默认安装到 `/usr/local/nginx` 目录。

创建符号链接:
```
ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin/
```

#### 配置nginx.conf
```
location ~ \.php$ {
    #fastcgi_pass   127.0.0.1:9000;
    fastcgi_pass   unix:/var/run/php-fpm.sock;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
```

## 安装PHP7

安装依赖:
```
apt-get install libxml2-dev libcurl4-gnutls-dev libjpeg-dev libpng-dev libmcrypt-dev libreadline6 libreadline6-dev libfreetype6-dev
```

缺少了openssl解决: 
```
ln -s /lib/x86_64-linux-gnu/libssl.so.1.0.0 /usr/lib/libssl.so
```

配置:
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-fpm --with-fpm-user=www-data --with-fpm-group=www-data --with-mysqli --with-pdo-mysql --with-iconv-dir --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring --with-mcrypt --enable-ftp --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --without-pear --with-gettext --disable-fileinfo --enable-maintainer-zts

编译:
```
make -j4
```

测试:
```
make test
```

安装:
```
make install
```

这样 PHP 将被安装到 `/usr/local/php` 目录。

编辑 `/etc/enviornment`, 将 `/usr/local/php/bin` 添加到 PATH 变量, 然后用 `source /etc/environment` 更新。

#### 配置 php.ini
```
cp <source-dir>/sapi/fpm/php.ini-development /usr/local/php/etc/php.ini
```

#### 配置php-fpm.conf
```
mv /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
```

#### 配置www.conf
```
mv /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
```
```
;listen = 127.0.0.1:9000
listen = /var/run/php-fpm.sock

listen.owner = www-data
listen.group = www-data
listen.mode = 0660

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20
pm.max_requests = 500
```

#### 初始化 php-fpm 服务脚本 
Reference: [FPM Installation and Configuration](http://php.net/manual/en/install.fpm.php)
```
cp <source-dir>/sapi/fpm/init.d.php-fpm.in /etc/init.d/php-fpm
chmod 755 /etc/init.d/php-fpm
```
修改该文件内容
```
prefix=
exec_prefix=
php_fpm_BIN=/usr/local/php/sbin/php-fpm
php_fpm_CONF=/usr/local/php/etc/php-fpm.conf
php_fpm_PID=/var/run/php-fpm.pid
```


## 安装MySQL

下载选择 community - source code - Generic Linux
```
wget -c http://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.10.tar.gz
```

boost库
```
apt-get install python-dev gccxml libbz2-dev
wget -c http://downloads.sourceforge.net/project/boost/boost/1.60.0/boost_1_60_0.tar.gz
tar -zvxf boost_1_60_0.tar.gz
cd boost_1_60_0/
./bootstrap.sh
./bjam --prefix==./prefix/install
./b2 install
```

```
apt-get install cmake bison libncurses5-dev
```

```
cmake \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/usr/local/mysql/data \
-DSYSCONFDIR=/etc \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_MEMORY_STORAGE_ENGINE=1 \
-DWITH_READLINE=1 \
-DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \
-DMYSQL_TCP_PORT=3306 \
-DMYSQL_USER=mysql \
-DENABLED_LOCAL_INFILE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DEXTRA_CHARSETS=all \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci

make && make install
```














