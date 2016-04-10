## 1 基于ubuntu的ARM开发环境搭建

### 1.1 虚拟机安装，网络环境配置

- 到[VirtualBox官网](https://www.virtualbox.org/) 下载免费的VirtualBox虚拟机软件。也可选择安装VMware Player软件。

- 到[Ubuntu官网下载区](http://www.ubuntu.com/download/alternative-downloads)下载Ubuntu 12.04.5 LTS DeskTop 镜像文件，根据具体的计算机硬件选择32位版本还是64位版本，硬件支持情况下尽量选64位版本。

> 从[周立功网站](www.zlg.cn/linux)也可以下载ubuntu镜像，与从ubuntu官方网站下载的镜像相比，该镜像已经安装好了Freescale的交叉编译环境。也可从该页面直接下载虚拟机文件在虚拟机软件中直接打开，就省掉了ubuntu安装过程。

- 在Virtualbox中安装Ubuntu。

### 1.2 网络设置：
Ubuntu安装完成后，Virtualbox缺省设置的网络设置为NAT模式，只要windows环境下可以正常上网，在ubuntu内可以直接连入互联网。
当需要调试嵌入式硬件时，通常需要把网卡设为桥接模式。
可以在虚拟机上配置双网卡，一块网卡用于连接互联网，一块网卡用于与嵌入式硬件实现局域网互联。

### 1.3 共享文件夹设置：

- 点击菜单 设备->安装增强功能，会弹出对话框要求输入密码。观察提示信息，等待安装完成。

- 在windows下建立一个共享目录，如在D盘建立 "D:\myshare"。

- 点击菜单 设备->共享文件夹，点击添加共享文件夹，
设置共享文件夹路径为 "D:\myshare"，命名一个共享文件夹名称如“vboxshare”，选中"固定分配"选项框，不选"只读分配"和"自动挂载"。

- 重启ubuntu。

- 在终端中输入：

```
sudo mount -t vboxsf vboxshare ~/share
```

即可实现共享文件夹的设置，实现在虚拟机下的ubuntu和windows共享文件的操作。

### 1.4 Git和代码下载

- [在线Git课程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

- 如果未安装git，首先安装git

```
sudo apt-get install git
```
```
git安装问题解决
sudo apt-get install git 出现依赖关系问题；

解决思路 ：更换 apt 软件源列表；
具体方法 ：
1. 首先备份源列表(for sure):
sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup
2.而后用gedit或其他编辑器打开:
gksu gedit /etc/apt/sources.list
3.选择合适的源，替换掉文件中所有的内容，保存编辑好的文件:
选用ubuntu 12.04官方软件源：
deb http://archive.ubuntu.com/ubuntu/ precise main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ precise-security main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ precise-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ precise-proposed main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ precise-backports main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ precise main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ precise-security main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ precise-updates main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ precise-proposed main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ precise-backports main restricted universe multiverse
4.然后，刷新列表:
sudo apt-get update
官网参考网站：
http://wiki.ubuntu.org.cn/Qref/Source
http://wiki.ubuntu.org.cn/Template:12.04source
```

Freescale的[Git网站](http://git.freescale.com/git/)，相关信息：
- Branch [分支](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000)
- Tag: [标签](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013762144381812a168659b3dd4610b4229d81de5056cc000)
- tree：当前主分支的最新文件目录
- commit：具体的提交信息
- diff：具体的补丁信息


### 1.5 交叉编译工具（环境变量）

从[周立功网站](www.zlg.cn/linux)下载的ubuntu已经包含了Freescale的交叉编译工具，如没有可以下载如下文件：
  

gcc-4.4.4-glibc-2.11.1-multilib-1.0.tar.bz2

### 1.6 QEMU

- [QEMU官网](http://wiki.qemu.org/Main_Page)

- 获取QEMU的最新版本：

```
git clone git://git.qemu-project.org/qemu.git
```
- 编译安装：

```
 cd qemu/
./configure
make
sudo make install
```

可能缺少的库，需要安装：

```
sudo apt-get install zlib1g-dev zlib1g-dbg libesd0-dev automake
sudo apt-get install libgtk2.0-dev
git submodule update --init dtc
```

安装完成后可运行 qemu-system-arm检查是否安装成功。
使用 -M 选项查看支持哪种体系结构

```
qemu-system-arm -M ?
```

其中:
- versatileab为： ARM Versatile/AB(ARM926EJ-S)
- versatilepb为： ARM Versatile/PB(ARM926EJ-S)


### 1.7 Yocto项目: 

[Yocto Project](https://www.yoctoproject.org)


## 2 ARM9和IMX287

### 2.1 ARM处理器架构和SOC产品

ARM的介绍参见[ARM公司网址](http://www.arm.com/zh/index.php)

[ARM处理器架构](http://www.arm.com/zh/products/processors/instruction-set-architectures/index.php)

ARM的主流处理器包括 Cortex-A系列、Cortex-R系列和Cortex-M系列。

ARM 经典处理器包括 ARM11、ARM9和ARM7处理器系列。这些处理器在全球范围内仍被广泛授权，为当今众多应用领域提供经济有效的解决方案。

ARM7 系列 - ARM7TDMI-S™ 和 ARM7EJ-S™ 处理器

ARM9 系列 - ARM926EJ-S™、ARM946E-S™ 和 ARM968E-S™ 处理器

ARM11 系列 - ARM1136J(F)-S™、ARM1156T2(F)-S™、ARM1176JZ(F)-S™ 和 ARM11™MPCore™ 处理器

ARM公司把ARM核授权给各个半导体公司，来生产ARM SOC产品，如：
  
  [NXP(含Freescale)产品](http://www.nxp.com/zh-Hans/products/microcontrollers-and-processors/arm-processors:ARM-ARCHITECTURE?lang_cd=zh-Hans&tid=FSH)
  
  [TI产品](http://www.ti.com.cn/lsds/ti_zh/dsp/embedded_processor.page)
  
  [ST产品](http://www2.st.com/content/st_com/zh/products/microcontrollers.html)
  
  [ATMEL产品](http://www.atmel.com/zh/cn/products/microcontrollers/default.aspx)
  
  FPGA与ARM的集成代表性器件: 
  [zynq-7000](http://china.xilinx.com/products/silicon-devices/soc/zynq-7000.html)
  
  
### 2.2 ARM926内核

[ARM926内核介绍](http://www.arm.com/zh/products/processors/classic/arm9/arm926.php)

MMU功能

浮点运算单元

### 2.3  IMX287 SOC简介

[iMX287介绍](http://www.nxp.com/zh-Hans/products/microcontrollers-and-processors/arm-processors/i.mx-applications-processors/i.mx28-processors/multimedia-applications-processors-dual-ethernet-dual-can-lcd-touch-screen-arm9-core:i.MX287?uc=true&lang_cd=zh-Hans)
- IMX287芯片的第一手资料： 官方手册 MCIMX28RM.pdf

- 地址空间分配

|片上设备   |开始地址    |结束地址    |长度 |
|---        |---         |---         |---  |
|片上RAM    |0x0000-0000 |0x0001-FFFF |128KB|
|外部存储器 |0x4000-0000 |0x7FFF-FFFF |1GB  |
|寄存器空间 |0x8000-0000 |0x800F-FFFF |     |
|片上ROM    |0xFFFE-0000 |0xFFFF-FFFF |128KB|

其中128K片内RAM为静态RAM，128K片内ROM固化了Freescale提供启动和诊断代码。

### 2.4  IMX287开发板

- 外部RAM： 128MB DDR-RAM，映射到0x4000-0000开始的地址空间。
- 闪存： 128MB NAND FLASH。

## 3 ARM9程序的交叉编译链接和仿真运行过程

### 3.1 ARM指令简介

### 3.2 交叉编译工具
  
  - 编译：arm-fsl-linux-gnueabi-gcc 
  - 汇编：arm-fsl-linux-gnueabi-gas
  - 链接：arm-fsl-linux-gnueabi-ld
  - 二进制文件处理：arm-fsl-linux-gnueabi-objcopy

> 扩展阅读： ELF文件格式；Makefile语法；LD脚本语言。

### 3.3 编译练习。

确认交叉编译器arm-fsl-linux-gnueabi-gcc已安装好。

编写entry.c文件：

```
volatile unsigned char* const UART0_PTR = (unsigned char*) 0x101f1000;

void print_uart0(const char * string)
{
    while (*string != '\0')
    {
        *UART0_PTR = *string;
        string++;
    }
}

int entry(void)
{
    print_uart0("Hello, HUST!\n");
    return 0;
}
```

编译：

```
arm-fsl-linux-gnueabi-gcc -g -c -mcpu=arm926ej-s entry.c -o entry.o
```

编写 startup.s文件：

```
    .global _MyApp
_MyApp:
	LDR sp, = stack_top
	BL entry
	B .
```

编译：

```
arm-fsl-linux-gnueabi-as -g  -mcpu=arm926ej-s startup.s -o startup.o
```

编写qemuboot.ld文件：

```
ENTRY ( _MyApp)
SECTIONS
{
  . = 0x10000;
  .startup . : {startup.o(.text)}
  .text : {*(.text)}
  .data : {*(.data)}
  .bss : {*(.bss COMMON)}
  . = ALIGN(8);
  . = . + 0x1000;
  stack_top = .;
}
```

链接生成目标文件qemuboot.elf：

```
arm-fsl-linux-gnueabi-ld -T qemuboot.ld entry.o startup.o -o qemuboot.elf
```

可以用readelf命令来读取qemuboot.elf的信息

进一步生成二进制文件：

```
arm-fsl-linux-gnueabi-objcopy -O binary qemuboot.elf qemuboot.bin
```

在qemu中运行：

```
qemu-system-arm -M versatilepb -nographic -kernel qemuboot.bin
```
> 练习：编写Makefile文件，完成上述编译过程。
参考：

```

CROSS_COMPILE = arm-fsl-linux-gnueabi-
AS	= $(CROSS_COMPILE)as
CC	= $(CROSS_COMPILE)gcc
LD	= $(CROSS_COMPILE)ld
OBJCOPY	= $(CROSS_COMPILE)objcopy
OBJDUMP	= $(CROSS_COMPILE)objdump

CFLAGS 	= 
LDFLAGS = 

BIN = boot.bin

all: $(BIN)

 $(BIN):
    @echo "generating a binary file from a ELF file"
    $(OBJCOPY)    
    
boot_elf:

    @echo "build a ELF file"
    $(LD)	

.PHONY: all clean

clean:
    @echo Cleaning...
    @echo Files:
    @echo Build output:
    rm -rf *.o

```

## 4 IMX287启动过程

### 4.1 存储器布局和启动过程分析

### 4.2 Freescale提供的启动代码

启动代码在imx-bootlets-src-10.12.01目录下

power_prep子目录：电源初始化代码。

boot_prep子目录： DDR初始化代码。

linux_prep子目录：Linux引导代码。

根目录下的Makefile如下：


```
export CROSS_COMPILE

CROSS_COMPILE = arm-fsl-linux-gnueabi-
BOARD = iMX28_EK
ARCH = mx28

all: gen_bootstream

gen_bootstream: boot_prep power_prep main_ivt.bd 
	@echo "generating boot stream image"

	elftosb -f imx28 -c ./main_ivt.bd -o imx28_ivt_uboot.sb
	

power_prep:
	@echo "build power_prep"
	$(MAKE) -C power_prep ARCH=$(ARCH) BOARD=$(BOARD)

boot_prep:
	@echo "build boot_prep"
	$(MAKE) -C boot_prep  ARCH=$(ARCH) BOARD=$(BOARD)

install:
	cp -f boot_prep/boot_prep  ${DESTDIR}

	cp -f power_prep/power_prep  ${DESTDIR}

	cp -f *.sb ${DESTDIR}

distclean: clean
clean:
	-rm -rf *.sb
	$(MAKE) -C boot_prep clean ARCH=$(ARCH)
	$(MAKE) -C power_prep clean ARCH=$(ARCH)

.PHONY: all  boot_prep power_prep distclean clean

```

main_ivt.bd文件

```
// STMP378x ROM command script to load and run U-Boot

sources {
	power_prep="./power_prep/power_prep";
	sdram_prep="./boot_prep/boot_prep";
}

section (0) {

	//----------------------------------------------------------
	// Power Supply initialization
	//----------------------------------------------------------

	load power_prep;
	load ivt (entry = power_prep:_start) > 0x8000;
	hab call 0x8000;

	//----------------------------------------------------------
	// SDRAM initialization
	//----------------------------------------------------------

	load sdram_prep;
        load ivt (entry = sdram_prep:_start) > 0x8000;
        hab call 0x8000;

}

```

运行 make all，生成imx28_ivt_uboot.sb



### 4.3 启动代码编译练习
修改boot_prep子目录下init-mx28.c的int _start(int arg)函数最后的 return 0;前添加几行自定义打印代码，如下：

```
//	printf("testDDRcode=%d\r\n",testDDRcode(1));
        printf("Welcome to HUST!\r\n ");
        printf("Enter Main Loop\r\n");
        while(1)
        {

        }
	return 0;
```

在imx-bootlets-src-10.12.01根目录下运行

```
make all
```

如无错误，将生成imx28_ivt_uboot.sb文件

### 4.4 烧写启动代码到开发板

- 把imx28_ivt_uboot.sb拷贝到windows下的MfgTool目录下的Profiles\MX28 Linux Update\OS Fireware\files目录中。
- 把开发板跳线改成USB启动（JP4,JP6短路），用USB电缆把开发板的USB OTG和电脑连接，开发板上电。
- 在Windows下打开MfgTool软件，点击主菜单Option->Configuration 菜单项，在Profiles下选择烧写NAND uboot Only，在USB Ports下勾选上已经连接上的“HID-compliant device”，回到主界面点击“开始”按钮开始烧写。
- 烧写完成显示成功后，开发板断电，把跳线恢复成NAND FLASH启动模式（JP4短路）。
- 重新启动开发板，在串口终端中观看运行结果。


## 附录 参考资源
大学课程  http://www.eecs.umich.edu/courses/eecs373/refs.html
