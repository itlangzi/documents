```ini
[Unit]
Description=php-fpm
After=network.target
[Service]
Type=forking
User=root
Group=root
ExecStart=/usr/local/php/bin/php-fpm
ExecReload=/usr/bin/kill -USR2 $MAINPID
ExecStop=/usr/bin/kill -s QUIT $MAINPID
# PrivateTmp=true
[Install]
WantedBy=multi-user.target
```