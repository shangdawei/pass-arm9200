# PASS ARM 9200 #

PASS ARM 9200开发板是基于AT91RM9200处理器的ARM9开发板，我们主要目的是提供廉价的嵌入式Linux入门学习平台。能够在简单的硬件平台下运行Linux操作系统，学习U-boot，Linux内核，Busybox移植以及网络开发、程序开发等方面的内容。

### Pass ARM 使用须知 ###
[specification](specification.md)

### 编译环境的建立 ###
[CrossCompileToolChain](CrossCompileToolChain.md)

### 焊接注意事项 ###
[Soldering](Soldering.md)

### 程序烧写 ###
[BinDownloading](BinDownloading.md)

### 内核编译 ###
[KernelPatches](KernelPatches.md)
使用内核 2.6.28.9，包含四个补丁

### 创建rootfs ###
[emdebian](emdebian.md)
生成一个基于emdebian的rootfs, 包括一个补丁脚本

### 调试过程中遇到问题请到邮件组或者Issues提问 ###
http://code.google.com/p/pass-arm9200/issues/list

### 邮件组 ###
要加入本项目，请在邮件组里留言
http://groups.google.com/group/pass-arm9200

### 项目发源地 ###
本项目的发源地及更多硬件开源项目
http://www.oshbbs.com