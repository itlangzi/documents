# Nginx+PHP7源码安装配置

原文: [Nginx+PHP7源码安装配置](https://www.yiibai.com/nginx/nginx-php7-source-config.html)

参考
https://segmentfault.com/a/1190000009909817  
https://www.php.net/manual/zh/install.unix.nginx.php  
```bash
./configure \
    --prefix=/usr/local/php \                                  [php安装的根目录]
    --exec-prefix=/usr/local/php \                               [php执行文件所在目录]
    --bindir=/usr/local/php/bin \                            [php/bin目录]
    --sbindir=/usr/local/php/sbin \                            [php/sbin目录]
    --includedir=/usr/local/php/include \                    [php包含文件所在目录]
    --libdir=/usr/local/php/lib/php \                        [php/lib目录]
    --mandir=/usr/local/php/php/man \                        [php/man目录]
    --with-config-file-path=/usr/local/php/etc \               [php的配置目录]
    --with-mysql-sock=/tmp/mysql.sock \           [php的Unix socket通信文件]
    --with-mhash \                                            [Mhash是基于离散数学原理的不可逆向的php加密方式扩展库，其在默认情况下不开启]
    --with-openssl \                                        [OpenSSL 是一个安全套接字层密码库]
    --with-mysqli=shared,mysqlnd \                          [php依赖mysql库]
    --with-pdo-mysql=shared,mysqlnd \                       [php依赖mysql库]
    --with-gd \                                                [gd库]                                                
    --with-iconv \                                            [关闭iconv函数，种字符集间的转换]                        
    --with-zlib \                                            [zlib是提供数据压缩用的函式库]
    --enable-zip \                                            [打开对zip的支持]
    --enable-inline-optimization \                            [优化线程]
    --disable-debug \                                        [关闭调试模式]
    --disable-rpath \                                        [关闭额外的运行库文件]
    --enable-shared \                                        [启用动态库]
    --enable-xml \                                            [开启xml扩展]
    --enable-bcmath \                                        [打开图片大小调整,用到zabbix监控的时候用到了这个模块]
    --enable-shmop \                                        [共享内存]
    --enable-sysvsem \                                        [内存共享方案]
    --enable-mbregex \                                        [开启多字节正则表达式的字符编码。]
    --enable-mbstring \                                        [开启多字节字符串函数]
    --enable-ftp \                                            [开启ftp]
    --enable-pcntl \                                        [PHP的进程控制支持实现了Unix方式的多进程创建]        
    --enable-sockets \                                        [开启套节字]
    --with-xmlrpc \                                            [打开xml-rpc的c语言]
    --enable-soap \                                            [开启简单对象访问协议简单对象访问协议]
    --without-pear \                                        [开启php扩展与应用库]
    --with-gettext \                                        [开户php在当前域中查找消息]
    --enable-session \                                      [允许php会话session]
    --with-curl \                                           [允许curl扩展]
    --with-jpeg-dir \                                        [指定jpeg安装目录yum安装过后不用再次指定会自动找到]
    --with-freetype-dir \                                    [指定freetype安装目录yum安装过后不用再次指定会自动找到]
    --enable-opcache \                                      [开启使用opcache缓存]
    --enable-fpm \                                            [开启fpm]
    --with-fpm-user=phper \                                 [php-fpm的用户]
    --with-fpm-group=phper \                                [php-fpm的用户组]
    --without-gdbm \                                        [数据库函数使用可扩展散列和类似于标准UNIX dbm的工作]
    --enable-fast-install \                                    [为快速安装优化]
    --with-gd  \                                             [验证码、缩略图]
```

```bash
make && make install
```
错误  configure: error: off_t undefined; check your library configuration 。
解决办法
```bash
echo '/usr/local/lib64
/usr/local/lib
/usr/lib
/usr/lib64'>>/etc/ld.so.conf&&ldconfig -v
```
> 参考： http://www.kwx.gd/PHPEnvironment/CentOS-PHP-off-t.html  


