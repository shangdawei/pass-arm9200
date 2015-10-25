# Linux环境下配置AT91RM9200固件下载工具 #
作者：郑明督

更新日志
2008.03.08首发布

tag:linux at91rm9200 kermit xmodem minicom uboot
最新文档http://docs.google.com/Doc?id=dc66rv46\_2mqcz29f9

如须转载请注明作者，并提供转载出处。

本文讲述Linux环境下AT91RM9200（以下简称9200）固件下载所使用的工具，及其编译配置，使用方法。
这些内容由Zoomdy的实际操作经验总结所得，主要针对Hyesco的开发板，但对其它板子，应该也是大同小异的，希望对朋友们有帮助，
第一次写，没太多经验，不足之处请多多指教 email: mingdu.zheng(at)gmail(dot)com。谢谢！
开发板:H9200E(Hyesco)
主机:Fedora 7

## 1.选择C-Kermit而不是minicom ##
给9200裸机下载固件需要使用xmodem协议发送文件到9200内部的SRAM中，minicom虽然也有xmodem组件，
但很不幸，minicom自身的xmodem组件与9200不兼容，无法使用minicom的xmodem组件下载固件到9200。
为了解决这个问题有高人编写了专门用于9200的xmodem程序，详情请看第2节。
此外uboot使用kermit协议来下载文件，minicom对kermit的支持同样存在问题，uboot官方使用手册也建议使用kermit而不是minicom。

## 2.准备工具C-Kermit和xmodem-at91 ##
到kermit网站 http://www.columbia.edu/kermit/ck80.html 下载C-Kermit，请下载Unix Complete版本
或者使用此链接ftp://kermit.columbia.edu/kermit/archives/cku211.tar.gz 直接下载

下载完成后解压
tar -xf cku211.tar.gz #如果你下载的是其它格式的压缩包，请使用相应的解压工具。
使用下面的命令编译
make linux
将wermit复制到~/bin目录下，也可以是/usr/bin, /usr/local/bin,或者其它，随你喜欢。
cp wermit ~/bin/kermit #复制的时候可以重命名一下，方便记忆。

下载xmodem-at91
http://www.koansoftware.com/it/art.php?art=68
使用此链接直接下载ftp://ftp.koansoftware.com/public/li...at91/sx-at91.c

修改串口设备名，sx-at91.c使用/dev/ttyS0作为串口设备，如果你的串口设备不是/dev/ttyS0,使用下面的命令编辑
sed -i 's!/dev/ttyS0!/dev/ttyUSB0!' sx-at91.c # 这里假设串口设备是/dev/ttyUSB0 ,USB转串口设备。
编译
gcc -o xmodem sx-at91.c
复制到~/bin目录下
cp xmodem ~/bin/xmodem

## 3.配置kermit ##
cat > ~/.kermrc << EOF
set line /dev/ttyS0 #根据您使用的串口设备修改此行
set speed 115200
set carrier-watch off
set handshake none
set flow-control none
robust
set file type bin
set file name lit
set rec pack 1000
set send pack 1000
set window 5
EOF

编辑/etc/group,将当前用户名添加到串口设备所属的组。Fedora 7下串口设备属于uucp组，将用户添加到uucp组里。
如果没有将当前用户添加到uucp组，将无法连接串口设备。

## 4.kermit的使用概要 ##
kermit有两种模式，一种为终端模式，一种为命令模式
处于终端模式时，显示从串口发回来的数据，处于命令模式时，显示命令提示符，并等待用户输入命令后，执行命令。
运行kermit，进入命令模式，输入"connect"并回车，进入终端模式。
在终端模式按下Ctrl + \, 再按下C 返回命令模式

|-- --- --| connect |--------|
|命令 |============>|终端 |
|模式 |<============|模式 |
|--------| Ctrl + \, c |--------|

常用命令
connect : 连接串口设备，连接成功后进入终端模式，简写为c
quit: 退出kermit， 简写为q
send: 使用kermit协议发送文件，与uboot传送文件时使用
run : 运行外部命令，我们将用这个命令调用xmodem发送文件。
? : 显示全部命令
! : 运行一个shell,需要临时离开kermit进行其它的作业的时候，可以使用叹号命令。结果操作时使用exit退出shell，返回kermit。


## 5.下载固件到裸机的SRAM中 ##
将9200目标板通过串口与主机连接，并将9200的BMS口线拉高，上电后，9200将从内部ROM启动。
运行kermit
kermit
连接设备
C-Kermit>connect
此时kermit进入终端模式，并不停地显示“C“字符，这是9200在等待主机发送固件到SRAM中。
按下Ctrl + \, 再按下c,返回命令模式
发送文件（loader.bin是H9200E开发板提供的固件，在software/uboot/bin目录下）
C-Kermit>run xmodem ~/loader.bin
xmodem下载文件到9200的SRAM中，完成后进入终端模式
C-Kermit>connect
您可以看到
loader 1.0 (Aug 8 2003 - 12:01:07)

XMODEM: Download U-BOOT
同时还会不停得出现“C“字符。
到此为止已经成功得将loader.bin下载到9200的SRAM中，并执行。

## 6.uboot的下载与安装 ##
将loader.bin下载到SRAM之后，就可以下载uboot.bin了，这是一个在SRAM中运行的uboot。
按下Ctrl + \, 再按下c,返回命令模式,将uboot.bin下载到SRAM中。
C-Kermit>run xmodem ~/uboot.bin
返回终端模式
C-Kermit>connect
您可以看到uboot的版本信息，及目标板的硬件配置，最后是一个uboot提示符。
U-Boot downloaded successfully


U-Boot 1.0.0 (Sep 25 2004 - 15:39:27)

U-Boot code: 21F00000 -> 21F1AA2C BSS: -> 21F26454
DRAM Configuration:
Bank #0: 20000000 32 MB
Fujitsu: 29LV320BE(32Mbit)
Flash: 4 MB
NAND:Entrying nand\_probe,break point1
Entrying NanD\_ScanChips
Entrying NanD\_IdentChip
mfr=ec
id=76
Flash chip found:
Manufacturer ID: 0xEC, Chip ID: 0x76 (Samsung K9F1208UOA)
1 flash chips found. Total nand\_chip size: 64 MB
64 MB
In: serial
Out: serial
Err: serial
Uboot>

现在目标板上运行的是临时的uboot固件，接下来将uboot安装到flash中。
擦除目标板上的flash
Uboot> protect off all
Uboot> erase all

下载boot.bin到目标板
Uboot>loadb 20000000
按下Ctrl + \, 再按下c,返回命令模式，使用kermit协议下载boot.bin
C-Kermit>send ~/boot.bin
下载完成后返回终端模式
C-Kermit>connect
将boot.bin复制到flash中
Uboot>cp.b 20000000 10000000 5fff
Uboot>protect on 10000000 10005fff

下载uboot.gz到目标板
Uboot>loadb 20000000
按下Ctrl + \, 再按下c,返回命令模式，使用kermit协议下载uboot.gz
C-Kermit>send ~/uboot.gz
下载完成后返回终端模式
C-Kermit>connect
将uboot.gz复制到flash中
Uboot>cp.b 20000000 10010000 ffff
Uboot>protect on 10000000 1001ffff

到这儿，已经将uboot安装到目标的flash中。将9200的BMS接低后，复位，就可以从flash加载uboot,并执行。
接下来的主角就是uboot啦，uboot支持以太网下载和串口下载，串口下载还使用kermit协议，上面已经有了详
细的使用kermit协议下载固件的方法，照搬就是啦。

### 参考 ###
ubuntu下使用kermit协议,通过串口传送文件 http://szricky.blog.hexun.com/8444684_d.html
AT91RM9200 xmodem upload tool http://www.koansoftware.com/it/art.php?art=68
C-Kermit 8.0 http://www.columbia.edu/kermit/ck80.html
The DENX U-Boot and Linux Guide (DULG) for TQM8xxL http://www.denx.de/wiki/DULG/Manual