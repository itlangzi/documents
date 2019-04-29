# Centos 7 升级内核  
## 安装yum源  
> 参考 http://elrepo.org/tiki/tiki-index.php  
```bash
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org   # 导入公共秘钥 
yum install https://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm # 安装 elrepo 的 yum 源
```

## 查看列表
```bash
# yum --disablerepo=* --enablerepo=elrepo-kernel repolist
# yum --disablerepo=* --enablerepo=elrepo-kernel list kernel*
yum --disablerepo="*" --enablerepo="elrepo-kernel" list available
```

### 安装最新版本的kernel
#### 安装内核
```bash
yum --enablerepo=elrepo-kernel install kernel-ml-devel kernel-ml -y
```
> 默认安装 mainline 版本, 也就是最新稳定版本

### 查看已安装的 Linux 内核版本
- 使用 `rpm -qa kernel*` 或 `rpm -qa | grep -i kernel` 命令

### 查找新安装的内核完整名称
- 使用 `cat /boot/grub2/grub.cfg | grep menuentry` 指令

### 切换默认启动内核
- 使用grub2-set-default '' 指令。（ 是上一步操作中复制的新内核名称，引号是不能少的）  
- 因为新安装的内核默认在第一位，所以使用grub2-set-default 0指令也是可以
```bash
grub2-set-default 'CentOS Linux (5.0.10-1.el7.elrepo.x86_64) 7 (Core)'
```
### 查看默认启动内核是否更改成功
- 使用grub2-editenv list命令
```bash
grub2-editenv list
```
### 重启验证
```bash
uname -r
```

## 卸载老版本的内核
> 要慎重操作  
> 建议只卸载自己手动安装的内核，不要动原来的内核
- 使用 `rpm -qa kernel*` 或 `rpm -qa | grep -i kernel` 命令，先找到内核版本号
```bash
rpm -qa | grep -i kernel
```
- 使用yum remove [版本号...版本号]命令卸载老版本的内核（最好是复制下来，别复制错了)

