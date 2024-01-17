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

## 配置bochs

```shell
cat > /opt/bochs/bochsrc.disk << EOF
# 能够使用的内存大小，单位MB
megs: 32

# 设置真实机器的BIOS和VGA BIOS
romimage: file=/opt/bochs/share/bochs/BIOS-bochs-latest
vgaromimage: file=/opt/bochs/share/bochs/VGABIOS-lgpl-latest

# 启动盘符
boot: disk

# 日志输出文件
log: bochs.out

# 关闭鼠标，打开键盘
mouse: enabled=0
keyboard_mapping: enable=1, map=/opt/bochs/share/bochs/keymaps/x11-pc-us.map

# 硬盘设置
ata0: enabled=1, ioaddr1=0x1f0, ioaddr2=0x3f0, irq=14

# 打开gdb到1234端口，因为编译没打开，所以此处不打开
# gdbstub: enabled=1, port=1234, text_base=0, data_base=0, bss_base=0
EOF
```

## 运行bochs

```shell
cd /opt/bochs
ln -s `pwd`/bin/bochs /usr/local/bin
bin/bximage -hd -mode="flat" -size=60 -q hd60M.img
# 会输出如下内容
# The following line should appear in your bochsrc:
# ata0-master: type=disk, path="hd60M.img", mode=flat, cylinders=121, heads=16, spt=63
# 复制到bochsrc.disk
echo "ata0-master: type=disk, path="hd60M.img", mode=flat, cylinders=121, heads=16, spt=63" >> bochsrc.disk

# 随后启动bochs
bochs -f bochsrc.disk
```
