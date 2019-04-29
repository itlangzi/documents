```ini
[Unit]
Description=frp is a fast reverse proxy to help you expose a local server behind a NAT or firewall to the internet
After=network.target

[Service]
Type=simple
User=nobody
ExecStart=/usr/local/frp/frpc -c /usr/local/frp/frpc.ini
ExecReload=/usr/local/frp/frpc reload -c /usr/local/frp/frpc.ini
ExecStop=/usr/bin/kill -s QUIT $MAINPID

[Install]
WantedBy=multi-user.target
```