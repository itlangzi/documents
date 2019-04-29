# Centos7 安装Nginx
## 创建nginx 用户
groupadd -r nginx
useradd -s /sbin/nologin -g nginx -M nginx

## 下载nginx 
```bash
wget https://nginx.org/download/nginx-1.16.0.tar.gz
```

## 解压
```bash
tar -zxvf nginx-1.16.0.tar.gz
```

## 安装依赖
```bash
yum install pcre pcre-devel openssl openssl-devel zlib zlib-devel
```
>- pcre库（支持rewrite模块）、zlib库（支持gzip模块）和openssl库（支持ssl模块）等。  
## 编译  
```bash
./configure --prefix=/usr/local/nginx  --user=nginx --group=nginx  --with-http_ssl_module --with-http_v2_module
make && make install
```