# Create an emdebian  rootfs #

Referring to:
http://www.emdebian.org/emdebian/rootfs.html


# 几个重点步骤 #

  1. 安装工具
> > 命令: apt-get install emdebian-tools
  1. 生成 rootfs
> > 命令: emsandbox
  1. 解压生成的tar.bz 包,拷到U盘上
  1. 下载 文件 /Downloads/fix\_emdebian.sh, 在U盘的根目录执行这个文件, 修正一些问题和加上root用户密码.
  1. U盘插入PASSARM, 如果一功顺利, 进入系统, 呵呵, 几乎不可能 <sup>_</sup> ......
  1. root用户密码janncker
  1. 执行./emsecondstage. 好了,一个能用的系统可以玩了....