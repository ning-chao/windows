# Windows计算机启动过程分析1

  **想要真正理解为什么winntsetup里的选项需要这样配置,需要了解Windows计算机启动流程.**

## 启动主板UEFI固件

  **按下开机键后,第一步便是启动主板UEFI固件.UEFI固件和Windows系统一点联系都没有,全名叫做 *统一扩展固件接口.***

  **UEFI取代了传统的BIOS固件,是时下所有新生产的电脑都配备的固件.一般来说,2015年以后的电脑都属于UEFI模式启动.**

  **在启动Windows前,计算机必须先启动UEFI固件.通过UEFI,计算机得以一步一步地引导Windows系统启动.**

> **只有Windows8及以后的Windows操作系统才支持UEFI固件.Windows7及以前的操作系统只支持BIOS固件.**

**Intel生产的UEFI固件启动菜单样式即为我们在VM中选择 "启动时进入固件" 看见的蓝色界面.这是UEFI的选择启动引导菜单界面.不同的生产商生产的UEFI界面不同,但功能类似.**

![871c7b858ad6b823cae9feb4d6cd7e3f.png](https://i.miji.bid/2024/03/01/871c7b858ad6b823cae9feb4d6cd7e3f.png)

  **对于我们来说,UEFI的主要作用就是*引导操作系统启动*.**

  **那么,UEFI是如何做到的呢?**

> **这就与系统引导分区(ESP分区)息息相关.**

## ESP分区

  **ESP分区,又名EFI分区,是一个存放操作系统启动文件的磁盘分区.EFI分区通常大小很小,约为200-300MB.**

  **值得注意的是,EFI分区的文件系统一般都是FAT32或FAT16类型!**

  **<u>因为一般的UEFI固件只能识别并读取FAT32和FAT16文件系统类型.</u>**

### EFI分区的文件目录

  **EFI分区拥有固定的文件目录,这都是为了迎合UEFI主板的口味.**

  **问题来了,如何才能打开EFI分区,看看分区里面的文件呢?**

  **EFI分区实际上属于操作系统的隐藏分区,而且保密级别高.不能通过 磁盘管理 分配新盘符,而是需要用第三方软件分配盘符.**

### DiskGenius磁盘管理工具

  **DiskGenius(简称DG)是一款优秀的老牌磁盘管理软件,由中国人开发.我们几乎可以使用DG完成一切能够对磁盘进行的操作.**

  **在这一回合中,我们通过DG给EFI分区一个盘符.(DG下载我在以前的文章里给过)**

**操作方法:**

    1.打开DG,在左侧找到ESP分区
    2.右键ESP分区,指派新的驱动器号(盘符)
    3.确定

![cf1c60a2ad09d987c43d46626e9595dd.png](https://i.miji.bid/2024/03/01/cf1c60a2ad09d987c43d46626e9595dd.png)
![4a3d3444943bcc9994c10f593ca843dc.png](https://i.miji.bid/2024/03/01/4a3d3444943bcc9994c10f593ca843dc.png)

  **这样,拥有了盘符的EFI分区就可以显示在文件资源管理器中了.**

![7b26730fc9c06f79b67963f6c7a2b36f.png](https://i.miji.bid/2024/03/01/7b26730fc9c06f79b67963f6c7a2b36f.png)

  **然而,操作系统心眼可是非常多的:)**

> **拒绝访问.**

  **操作系统也是好心好意,毕竟里面的文件极其重要,弄不好直接无法启动电脑.**

  **但是在虚拟机里,无法启动又算得了什么呢?:)**

> **上文提到的方法在PE系统中适用,PE系统可以无视各种权限,各种分区任你宰割.**

  **我们可以直接使用DG软件查看EFI分区里的文件.**

  **在DG里双击ESP分区,或者在右侧面板 浏览文件 都可以查看.你甚至可以右键一个文件或文件夹,复制到桌面.**

  **EFI分区的目录如下:(无需掌握)**

![fd23224dde72ddf568c52124134a022b.gif](https://i.miji.bid/2024/03/01/fd23224dde72ddf568c52124134a022b.gif)

    EFI
        ├─Boot
        └─Microsoft
            ├─Boot
            │  ├─bg-BG
            │  ├─cs-CZ
            │  ├─da-DK
            │  ├─de-DE
            │  ├─el-GR
            │  ├─en-GB
            │  ├─en-US
            │  ├─es-ES
            │  ├─es-MX
            │  ├─et-EE
            │  ├─fi-FI
            │  ├─Fonts
            │  ├─fr-CA
            │  ├─fr-FR
            │  ├─hr-HR
            │  ├─hu-HU
            │  ├─it-IT
            │  ├─ja-JP
            │  ├─ko-KR
            │  ├─lt-LT
            │  ├─lv-LV
            │  ├─nb-NO
            │  ├─nl-NL
            │  ├─pl-PL
            │  ├─pt-BR
            │  ├─pt-PT
            │  ├─qps-ploc
            │  ├─Resources
            │  │  ├─en-US
            │  │  └─zh-CN
            │  ├─ro-RO
            │  ├─ru-RU
            │  ├─sk-SK
            │  ├─sl-SI
            │  ├─sr-Latn-RS
            │  ├─sv-SE
            │  ├─tr-TR
            │  ├─uk-UA
            │  ├─zh-CN
            │  └─zh-TW
            └─Recovery

  **为什么说,EFI分区里的文件目录的这种结构不可以改变呢?**

  **这就要介绍一种新的文件类型:efi文件.**

### *.efi文件

  **efi文件可以被理解为UEFI系统里的软件.(很像exe)efi文件也是一种程序.**

  **efi程序作用是引导操作系统启动.UEFI固件在启动计算机过程中一个重要的任务便是寻找到efi程序,并把efi程序列出在系统引导菜单里.**

  **efi程序被放在EFI引导分区里,由于EFI分区是FAT32或FAT16文件系统,UEFI固件可以识别并访问EFI分区里的文件.**

  **常见的efi程序包括:**

    1.bootmgfw.efi
    2.bootmgr.efi
    3.memtest.efi
    4.bootx64.efi

  **1.bootmgfw.efi:笔者以为是boot manager for Windows的缩写.因为这个efi程序是专门启动Windows系统用的.**

  **2.bootmgr.efi:可能是boot manager的简写,是在bootmgfw.efi丢失或损坏时的备用程序.**

  **3.bootx64.efi:最为神通广大的efi引导启动程序,可以引导除了Windows以外的其他操作系统,比如Linux.**

  **4.memtest.efi:计算机内存诊断工具.**

  **上文说到,UEFI固件把efi程序列在系统引导菜单里.那么,UEFI是如何寻找到这些efi程序的呢?**

  **EFI分区的目录结构之所以不能改变,原因就在于此.UEFI固件"记住"了EFI分区的目录结构.所有的生产厂家都需要按照这个目录结构存放自己的EFI分区文件.**

  **也就是说,UEFI固件寻找bootmgfw.efi程序的过程是这样的:**

    1.扫描EFI分区(文件系统为FAT32或FAT16的分区)
    2.进入EFI分区,按照 EFI->Microsoft->Boot->bootmgfw.efi的顺序寻找efi程序
    3.找到后,把bootmgfw.efi所包含的信息列在UEFI引导菜单上(叫做Windows boot manager)

![d749dcaeb97b9c982970e371479f87ae.png](https://i.miji.bid/2024/03/01/d749dcaeb97b9c982970e371479f87ae.png)

  **在下一节,我们将具体介绍bootmgfw的工作过程.**
