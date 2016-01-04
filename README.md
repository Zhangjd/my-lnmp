# my-lnmp

个人lnmp配置

## 系统配置

Linux: Ubuntu 14.04
PHP: 7.0.1
Nginx: 1.9.9
MySQL: 

### 安装Nginx

依赖关系:
```
apt-get install build-essential libtool libpcre3 libpcre3-dev openssl libssl-dev
```

配置:
```
./configure --user=www-data --group=www-data --with-http_stub_status_module --with-http_ssl_module --with-file-aio --with-http_realip_module  
```

安装:
```
make && make install
```

Nginx默认安装到 `/usr/local/nginx` 目录。

创建符号链接:
```
ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin/
```

### 安装PHP7

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


### 安装MySQL















