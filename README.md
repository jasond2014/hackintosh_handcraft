
[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu)
[![LICENSE](https://img.shields.io/badge/license-Anti%20996-blue.svg)](https://github.com/996icu/996.ICU/blob/master/LICENSE)

记录和分享配置好的Hackintosh EFI。

**使用必知**：请使用Clover Configurator在SMBIOS中替换一个新的Serial Number以“洗白”。同时也可以避免多账户使用同一个Serial Number被苹果封锁Apple ID。

```
Home Desktop   : i3-8400, ASUS B360M-A, G.Skill DDR4, Yeston RX 570 GAMEACE 4G, macOS10.15.7
Home Laptop    : Dell Latitude 5490, i5-8250U, macOS10.15.4
Office Desktop : i3-8100, Onda H310 ITX, Onda DDR3, Intel 540s M.2 Sata, macOS10.15.4
Former Laptop  : Thinkpad T450, i5-5300u, 8G*2 DDR3L, mSata 256G, macOS10.15.3 (deprecated)
Former Desktop : i3-8100, MSI B360M Fire, 8G*2 DDR4 corsair, Yeston RX570 4G, Intel 760p M.2 NVMe, macOS10.15.2 (deprecated)
Former Desktop : Dell 9020m, i5-4670T, 4G*2 DDR3L Samsung, Samsung 850 EVO, macOS10.15.2 (deprecated)
```

### mac_utils 文件夹
- HWSensors.6.26.1800.zip: 检测硬件健康、温度、风扇转速等的状态栏工具，小而且好用，需要kexts驱动和app一起使用。
- iStatusMenus: 另一个状态栏监测工具，mac_utils里面有方法**。
- Office 2019下载地址及**包。
- fliqlo: 时钟屏保。
- RTLWlanU_MacOS10.11_MacOS10.15_Driver_1830.32.b13_1827.4.b36_UI_5.0.9.b6.zip: 1个USB WiFi驱动。
- CPU-S: 变频测试。
- IntelPowerGadget: Intel官方硬件监测软件。
- neofetch: 查看系统spec，使用pip安装。
- 查看cpu核心数: `sysctl hw.physicalcpu`, `sysctl hw.logicalcpu`
- 查看空余内存: `alias free="top -l 1 -s 0 | grep PhysMem"`

### Hackintosh Tips
- 常识：安装系统文件一般不麻烦，打驱动(声卡、显卡、网卡、USB)可能会很麻烦；笔记本一般比台式机麻烦多很多。找贴近官方设备的硬件，借用已构建好的EFI比较省事。据说可以直接拷贝系统文件完成安装，未尝试过。
- 入门安装方法：按照tonymacx86论坛的[Install Guide](https://www.tonymacx86.com/threads/unibeast-install-macos-mojave-on-any-supported-intel-based-pc.259381/)，配置好BIOS后，去AppStore下载系统镜像(或者下载dmg安装成.app)，使用Unibeast制作安装U盘，走完安装流程后，使用Multibeast完成Post Install的驱动安装和config配置。
- 其他安装方法：使用命令行createvolumn到u盘，自动生成EFI分区，把Clover安装到EFI分区，然后去安装。
- Clover：是用来引导MacOS系统的，可以同时引导Windows和MacOS双系统。大版本MacOS系统更新有时会需要把Clover升级到较新的版本才能无缝OTA升级。可以直接[使用clover新版安装文件](https://www.bilibili.com/video/av49751074?t=72)来升级（注意选项谨防失败），也可以简单[替换CLOVERX64.efi文件](https://blog.zuiyu1818.cn/posts/Hac_Clover.html)。
- CLover Configurator：即CCG，通过plist文件配置引导细节，更新Clover和kexts驱动。一般要先加载EFI所在分区。也可以用`diskutil list`查看引导所在EFI分区，`diskutil mount`进行加载。
- kexts：硬件驱动程序。Multibeast默认会把kexts安装到/Library/Extensions/(/L/E)系统目录。为了系统更新后kexts不丢失，我把所有kexts驱动搬迁到/EFI/CLOVER/Kexts/other目录，即驱动和引导放在一起。
- kexts缓存重建：kexts更新后可以用指令`sudo kextcache -i /`操作刷新驱动缓存。
- 声卡：一般去AppleALC的[wiki](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs)里面找你的声卡对应的layout_id，写到Devices->Audio->Inject里面。我用过的2块主板都是ALC887，用的11。
- 显卡：核显可以找小兵大大的博客，使用Hackintool打显卡的patch。AMD RX4xx/RX5xx系列可以直接WhateverGreen.kext+Lilu.kext进行驱动。
- USB：一般USBInjectAll.kext直接可以驱动，不行的可以试试在Acpi->Patches里面加上`change EHC1 to EH01` `change EHC2 to EH02`，在Kernel_and_kexts_patches里面加上usb port limit patch。
- 网卡：
  - 有线一般放一个相应厂商的kexts就能驱动。
  - USB WIFI 我用的是CF-811AC，[官方最新驱动](http://www.comfast.cn/index.php?m=content&c=index&a=show&catid=30&id=396)支持catalina，也使用开源的[Wireless USB Adapter Clover](https://github.com/chris1111/Wireless-USB-Adapter-Clover/releases)软件驱动。
  - 仅有m.2接口的博通网卡才能支持airdrop等，如果确实有需要，可以选择Mac拆机网卡BCM94360CS2等。会需要加入一些kexts或修改config文件，同时好像也有一定几率失败，具体可以参考小兵的blog。
- OC vs Clover: 主要功能上Clover老当益壮，多系统支持更好。
