# 搬瓦工安装shadowsocks Python3版本
>- `shadowsocks` 源码: [shadowsocks](https://github.com/shadowsocks/shadowsocks "shadowsocks")
>- `shadowsocks wiki`: [shadowsocks wiki](https://github.com/shadowsocks/shadowsocks/wiki "shadowsocks wiki")
>- 重新安装系统 `Centos` 安装带有 `bbr` 的版本对网络有优化

***注意***
>- 搬瓦工默认也支持 `shadowsocks` 一键安装 只不过被隐藏了
>- 购买完成并登录 在同一浏览器输入链接 [https://kiwivm.64clouds.com/main-exec.php?mode=extras_shadowsocks]( "shadowsocks安装")
>- 目前默认安装支持 `Centos 6 (32 or 64 bit)`, `Python2`


## 安装Python3
> 最新版本 `shadowsocks` 依赖 `Python3`

一、安装必要的编译工具
```bash
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
```
二、下载最新版本的 `Python3`
```bash
wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tar.xz
```
> 1. 若没有 `wget` 命令，安装即可 `yum install wget -y`
> 2. 默认下载位置在当前用户目录下,执行以下命令查看
```bash
cd ~
ls -ls
```
三、解压并安装
```bash
tar -xvJf Python-3.7.2.tar.xz
cd Python-3.7.2
./configure --prefix=/usr/local/python3
make && make install
```

四、创建软链接，是系统默认使用`Python3`
```bash
mv /usr/bin/python /usr/bin/python.bak  # 备份原来的
mv /usr/bin/pip  /usr/bin/pip.bak       # 备份原来的
ln -s /usr/local/python3/bin/python3 /usr/bin/python #将python3设置成默认
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip
python -V   # 查看版本是否正确
pip -V      # 查看版本是否正确
```

五、注意：
> 若此时`yum`（或其它工具）不能使用，报错
> 执行以下命令
```bash
vim /usr/bin/yum
#!/usr/bin/python
.....
```
> 将第一行改成 `#!/usr/bin/python2.7`  **其它工具类似**


## 安装 `shadowsocks`
一、从 `Github` 上安装最新版本的 `shadowsocks`
```bash
pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```
> 安装 `Git` 命令 `yum install git`

二、若没有`ssserver`命令，看此步骤
>添加软链接可以直接使用 `ssserver` 命令,`shadowsocks` 使用`pip`安装后默认的`ssserver`命令在`Python3`的`bin` 目录下
```bash
ln -s /usr/local/python3/bin/ssserver /usr/bin/ssserver
```

三、添加配置文件
```bash
vim touch
```
添加如下内容
1、单用户
```json
{   
  "server":"<IP>",
  "local_address":"127.0.0.1",
  "local_port":443,
  "server_port":8888,
  "password":"<password>",
  "timeout":"10",
  "method":"aes-256-cfb"
}
```
>- `server` 是**服务器的`IP`**
>- `local_address` 默认是 `127.0.0.1`
>- `local_port` 默认 `443`
>- `server_port` 自定义端口,供客户端链接使用
>- `password` 密码,供客户端链接使用
>- `method` 加密方式 建议采用 `aes-256-cfb`
>- 使用`aes-256-cfb`加密建议安装 `m2crypto` 可加快加解密的速度 命令 `yum install m2crypto`

2、多用户
> 去掉单用户中`server_port`和`password`换成`port_password`

```json
{
  "server":"<IP>",
  "local_address":"127.0.0.1",
  "local_port":443,
  "port_password":{
    "<port1>":"<password1>",
    "<port2>":"<password2>",
    "<port3>":"<password3>"
  },
  "timeout":"10",
  "method":"aes-256-cfb"
}
```
> 支持动态添加用户 详见 [Manage Multiple Users](https://github.com/shadowsocks/shadowsocks/wiki/Manage-Multiple-Users "wiki")

四、`ssserver` 命令介绍
1. **后台运行**启动、停止、重启

```bash
ssserver -c /etc/shadowsocks.json -d <start|stop|restart> --log-file=/tmp/shadowsocks.log
```
> 示例启动
> `ssserver -c /etc/shadowsocks.json -d start --log-file=/tmp/shadowsocks.log`
2. 不作为后台运行 去掉 ` -d <start|stop|restart> ` 参数即可

## 附录
搬瓦工默认的防火墙未启用，**危险**
### 防火墙操作
> 由于我们修改`Python`默认版本,有可能此命令会执行失败,修复的方法同上
```bash
vim /usr/bin/firewall-cmd
#!/usr/bin/python -Es
.....
```
```bash
vim /usr/sbin/firewall
#!/usr/bin/python -Es
.....
```
> 将第一行的内容改成 `#!/usr/bin/python2.7 -Es` 即可
1. 启动防火墙
```bash
systemctl start firewalld.service
```
2. 关闭防火墙
```bash
systemctl stop firewalld.service
```
3. 防火墙开机自启
```bash
systemctl enable firewalld.service
```
4. 添加端口
```bash
firewall-cmd --permanent --zone=public --add-port=22/tcp
```
5. 删除端口
```bash
firewall-cmd --permanent --remove-port=22/tcp
```
> `--permanent` 参数表示永久生效
6. 查看开放的端口
```bash
firewall-cmd --permanent --zone=public --list-ports
```
7. 查看开放的服务
```bash
firewall-cmd --permanent --zone=public --list-services
```

8. 添加或删除端口后需要重新加载防火墙
```bash
firewall-cmd --reload
```



