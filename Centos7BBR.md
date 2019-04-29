# `Centos7` `BBR` 加速 
> `TCP` `BBR` 是谷歌出品的 `TCP` 拥塞控制算法。BBR目的是要尽量跑满带宽，并且尽量不要有排队的情况。`BBR` 可以起到单边加速 `TCP` 连接的效果  
> TCP-BBR的目标就是最大化利用网络上瓶颈链路的带宽。一条网络链路就像一条水管，要想最大化利用这条水管，最好的办法就是给这跟水管灌满水  
> BBR解决了两个问题：
- 在有一定丢包率的网络链路上充分利用带宽。非常适合高延迟，高带宽的网络链路。
- 降低网络链路上的buffer占用率，从而降低延迟。非常适合慢速接入网络的用户。

##  升级内核，第一步首先是升级内核到支持BBR的版本：
- 查看系统版本

https://www.lefer.cn/posts/42384/
# 更新系统
yum update -y
# 安装内核
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install kernel-ml
# 查看安装内核并设置，从返回结果中找到版本号最大的一行的序号，设置为默认启动
awk -F\' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg
# 0是版本号最大的一行的序号
grub2-set-default 0
# 重启
reboot
# 查看内核
uname -a
# 开启BBR
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
# 保存并生效
sysctl -p 
# 查看是否开启。返回值有 tcp_bbr 模块即说明 bbr 已启动。
lsmod | grep bbr

# 备注
出现错误  
```bash
sysctl: cannot stat /proc/sys/net/ipv4/tcp_tw_recycle: 没有那个文件或目录
```
修改文件  
```bash
vim /etc/sysctl.conf
```
注释或删除 `net.ipv4.tcp_tw_recycle`  
```vim
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
# net.ipv4.tcp_tw_recycle = 1
net.ipv4.ip_local_port_range = 1024 65000
net.ipv4.tcp_max_syn_backlog = 8192
net.ipv4.tcp_max_tw_buckets = 5000
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
```
再次执行 
```bash
sysctl -p
```