# 内核下载 #

直接从kernel.org 下载：
http://kernel.org/pub/linux/kernel/v2.6/linux-2.6.28.9.tar.bz2


# 几个补丁文件的说明 #

补丁包在/Downloads/patches.tar.bz2, 包含4个文件：
  1. 0001-at91-patch.patch
  1. 0002-add-.config-file.patch
  1. 0003-fix-unrecognized-machine-ID-error.patch
  1. 0004-fix-the-no-mac-issue-after-burned-to-flash.patch

几点说明:

  1. 0001 是公板AT91RM的补丁,这个必需的
  1. 0002 是一个配置好的.config文件, 你也可以自己再改,可选
  1. 0003 修正了一个passarm与公板AT91RM之间不同而导致的问题
  1. 0004 修正了kernel\_img烧到FLASH 之后,MAC地址没有配置的问题

这四个补丁要按次序一个一个打上.

打补丁命令: patch  -p1 < 000xxx. 要在kernel的根目录.