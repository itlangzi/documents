```ini
[Unit]
Description=Gogs
After=syslog.target
After=network.target
# After=mariadb.service mysqld.service postgresql.service memcached.service redis.service

[Service]
# Modify these two values and uncomment them if you have
# repos with lots of files and get an HTTP error 500 because
# of that
###
# LimitMEMLOCK=infinity
# LimitNOFILE=65535
Type=simple
User=gogs
Group=gogs
WorkingDirectory=/usr/local/gogs
ExecStart=/usr/local/gogs/gogs web
ExecStop=/usr/bin/kill -s QUIT $MAINPID
# Restart=always
# Environment="USER=gogs",HOME="/usr/local"

# Some distributions may not support these hardening directives. If you cannot start the service due
# to an unknown option, comment out the ones not supported by your version of systemd.
# ProtectSystem=full
# PrivateDevices=yes
# PrivateTmp=yes
# NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
```

注意: 源码安装的 `Git`; 需要在`/bin/git` 能够找到 否则无法启动  
添加软连接 
```bash
ln -s /usr/local/git/bin/git /bin/git
```