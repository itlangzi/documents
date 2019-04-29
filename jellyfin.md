# 个人媒体Jellyfin安装
> `Jellyfin` 是 `Emby 3.5.2` 后续版本；`Emby3.5` 之后闭源     
> 以 `Centos7` 为例  
- 下载 `RPM` 包 [https://github.com/jellyfin/jellyfin/releases](https://github.com/jellyfin/jellyfin/releases 'RPM')  
- 安装依赖  
```bash
yum install libicu fontconfig -y
```
- 安装软件  
```bash
rpm -Uvh --nodeps https://repo.jellyfin.org/releases/server/centos/jellyfin-10.2.2-1.el7.x86_64.rpm  
```
- 启动jellyfin
```bash
systemctl start jellyfin
```
- 查看状态
```bash
systemctl status jellyfin
```
- `CentOS 7` 开机自启
```bash
systemctl enable jellyfin
```
- `CentOS 7` 关闭开机自启
```bash
systemctl disable jellyfin
```
- 然后通过 `ip:8096` 访问该媒体库

> 一般 `CentOS` 是没安装 `ffmpeg` 的，先使用命令 `ffmpeg -version` 检查下 `ffmpeg` 是否存在，不存在就安装  
> 参考 [https://www.jianshu.com/p/94a1759ceb34](https://www.jianshu.com/p/94a1759ceb34)
- Nasm
```bash
wget https://www.nasm.us/pub/nasm/releasebuilds/2.14/nasm-2.14.tar.gz
tar -zxvf nasm-2.14.tar.gz
cd nasm-2.14
./autogen.sh
./configure --prefix="/usr/local/ffmpeg" --bindir="/usr/bin"
make
make install
```
- Yasm
```bash
wget http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz
tar -zxvf yasm-1.3.0.tar.gz
cd yasm-1.3.0
./configure --prefix="/usr/local/ffmpeg" --bindir="/usr/bin"
make
make install
```
- libx264
```bash

```
- FFmpeg
```bash
# 下载ffmpeg二进制
wget http://www.ffmpeg.org/releases/ffmpeg-4.1.tar.gz
# 解压文件
tar -zxvf ffmpeg-4.1.tar.gz
cd ffmpeg-4.1
./configure \
  --prefix="/usr/local/ffmpeg" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I/usr/local/ffmpeg/include" \
  --extra-ldflags="-L/usr/local/ffmpeg/lib" \
  --extra-libs=-lpthread \
  --extra-libs=-lm \
  --bindir="/usr/bin" \
  --enable-gpl \
  --enable-libfdk_aac \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-nonfree
make
make install
```

> 最后安装好了，就自行去后台设置，转码那里还需要你填上ffmpeg路径，一般为/usr/bin，可使用which ffmpeg查看路径
