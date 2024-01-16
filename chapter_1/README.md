# 第一章

## 安装bochs

bochs2.6.2，源码下载地址：[https://sourceforge.net/projects/bochs/files/bochs/2.6.2/](https://sourceforge.net/projects/bochs/files/bochs/2.6.2/)，下载bochs-2.6.2.tar.gz

```shell
mv bochs-2.6.2.tar.gz /root/bochs
cd /root/bochs
tar -zxvf bochs-2.6.2.tar.gz
cd bochs-2.6.2
./configure \
--prefix=/opt/bochs \ 
--enable-debugger \ 
--enable-disasm \ 
--enable-iodebug \ 
--enable-x86-debugger \
--with-x \ 
--with-x11
# 如果提示 ERROR: X windows gui was selected, but X windows libraries were not found.
# 则安装：apt-get install libx11-dev xserver-xorg-dev xorg-dev
make
# 如果提示 gtk_enh_dbg_osdep.cc:20:10: fatal error: gtk/gtk.h: No such file or directory
# 则安装：apt-get install libgtk2.0-dev 或者 apt-get install libgtk-3-dev
# 然后再执行configure，再make，再make install
make install
```

```shell
--prefix=/opt/bochs  #设置安装目录
--enable-debugger #开启bochs调试器
--enable-disasm  #支持反汇编
--enable-iodebug  #启用io接口调试器 
--enable-x86-debugger  #支持x86调试器
--with-x  #使用x windows
--with-x11 #使用x11图形用户接口
```

最后`/opt/bochs/bin/bochs  --help`可打印bochs版本等信息
