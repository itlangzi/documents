```ini
[Unit]
Description=nginx - high performance web server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/usr/local/nginx/pid/nginx.pid
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop

[Install]
WantedBy=multi-user.target
```

`systemctl` 服务脚本存放在：`/usr/lib/systemd/`，有系统（`system`）和用户（`user`）之分，

`/usr/lib/systemd/system/ `  
`/usr/lib/systemd/user/`  

- 开机启动`nginx `   
```bash
systemctl enable nginx.service
```
- 禁止开机启动`nginx`  
```bash
systemctl disable nginx.service
```

- 启动`nginx`  
```bash  
systemctl start nginx.service
```

- 停止`nginx`  
```bash
systemctl stop nginx.service
```

- 重新加载`nginx`
```bash  
systemctl reload nginx.service
```

- 查看`nginx`状态  
```bash
systemctl status nginx.service
```
# 权限
创建 `nginx` 用户组和用户  
```bash
groupadd nginx  
useradd -g nginx -M nginx  
```
> `useradd`命令 `-g` 表示用户组
> `useradd` 命令的 `-M` 参数用于不为 `nginx`建立 `home` 目录

```bash
userdel nginx 
groupdel nginx
usermod –G nginx nginx  //（强制删除该用户的主目录和主目录下的所有文件和子目录）

```

修改/etc/passwd，使得nginx用户无法bash登陆(nginx用户后面由/bin/bash改为/sbin/nologin)
```bash
vim /etc/passwd
```
然后找到有 nginx 那一行，把它修改为(后面由/bin/bash改为/sbin/nologin)：
```ini
nginx:x:1001:1002::/home/nginx:/bin/bash:/sbin/nologin
```




修改配置文件  
```bash
vim /usr/local/nginx/conf/nginx.conf  
```
第一行加入 `user nginx nginx;`
```ini
user nginx nginx;
worker_processes  1;
...
```

将nginx目录的权限赋给nginx用户
```bash
cd /usr/local/nginx
chown nginx:nginx
```
