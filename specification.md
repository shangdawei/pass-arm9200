# 特别说明 #
本文档是以哈尔滨博兴电子科技发展有限公司的PASS9200 ARM9开发平台为基础进行移植开发，可能与其他硬件平台有所不同，如果在移植开发过程中遇到问题，请在ISSUEhttp://code.google.com/p/pass-arm9200/issues/list提问，或请加入qq群：52372111进行讨论(推荐使用前者，促进项目发展)。由于时间、精力等方面问题，本公司不提供技术支持，望大家谅解。

关于版权：开放本文所有文字的转载权，但请注明出处。但我们保留结集出版的权利。

关于本工程：本工程是基于PASS9200 ARM平台的开源(Open Source)项目，所有个人可以自由(Free)下载使用。欢迎加入本项目，并参与维护。哈尔滨博兴电子科技发展有限公司保留本项目的商业权利。

# 前言 #
PASS ARM 9200开发板是基于AT91RM9200处理器的ARM9开发板，我们主要目的是提供廉价的嵌入式Linux入门学习平台。能够在简单的硬件平台下运行Linux操作系统，学习U-boot，Linux内核，Busybox移植以及网络开发、程序开发等方面的内容。
PASS ARM 9200学习板调试开发环境有两种方式：
  * 可以采用一台PC运行虚拟机的方式，即Windows+vmware+Linux 方式，需要的配置较高的PC即可。
  * 另一种方式采用两台PC，一台安装Windows，另一台安装Linux。在Windows下修改文件以及下载程序，在Linux下主要用来编译程序。
本文主要介绍的是使用两台PC的方式，同时我们还会介绍Linux交叉编译环境，arm-linux-gcc等的基本应用。

# 开发平台简介 #
ATMEL AT91RM9200介绍
Atmel公司的AT91RM9200是基于ARM Thumb的ARM920T微控制器，时钟频率为180MHz，运算速度可以达到200MIPS。它有丰富的系统与应用外设及标准的接口，从而为低功耗，低成本，高性能的计算机宽范围应用提供一个单片解决方案。
AT91RM9200主要特性：
  * 融合了ARM920T™ ARM® Thumb® 处理器；
  * 支持SDRAM，静态存储器， Burst Flash，无缝连接的CompactFlash，SmartMedia及NAND Flash10/100 Base-T 型以太网卡接口；
  * USB 2.0 全速(12 M比特/秒) 主机双端口；
  * USB 2.0 全速(12 M比特/秒) 器件端口；
  * 多媒体卡接口(MCI)，MMC及SD存储器卡兼容，支持两个SD存储；
  * 3个同步串行控制器(SSC)；
  * 4个通用同步/异步接收/发送器(USART)；
  * 主机/从机串行外设接口(SPI)；
  * 两个 3 通道16 位定时/计数器(TC)。

# PASS ARM 9200学习板特性 #
  * CPU：AT91RM9200。
  * 主频180M,总线60M。
  * FLASH：SST公司的SST39VF6401B，单片，16bit总线，8MB容量。也支持1601/3201系列。
  * SDRAM：Hynix 公司的HY57V641620HG，双片，32bit总线，容量16MB,可扩展为64MB。支持HY56系列。
  * 以太网：Davicom公司的DM9161E，10M/100M自适应。
  * USB HOST：2.0 Full Speed，标准USB A口。
  * USB Device：2.0 Full Speed，标准USB B口。
  * DeBuG Com：调试下载DB9 COM公口。
  * 电源：外接DC9V@500mA供电，或USB供电。
  * 其他扩展接口：RS-232（标准电平、TTL电平），SPI，I2C。
其他详细资料请查看PASS ARM 9200学习板的原理图。