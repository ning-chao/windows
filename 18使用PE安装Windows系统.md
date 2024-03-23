# 使用PE安装Windows系统

  **使用PE安装Windows系统是一种较为高级的系统安装方法,通过这种方法安装操作系统,你不仅可以体验到真正的Windows系统开箱过程,还能逃过微软官方的TPM审查,使不符合要求的电脑机型也可以使用高版本的Windows.**

  **本文将简单介绍如何使用PE安装操作系统,其内部原理在未来会详细讲解.**

> **请不要用自己的物理机做实验!请使用VM虚拟机在PE里安装Windows!**

**设置情境:**

    假如你在第九篇文章里安装的Windows10系统已经损坏,需要使用PE系统格式化它的C盘以重新安装一个新的Windows10.

**操作方法:**

    1.向虚拟机里添加一个虚拟光盘,选择微PE的ISO文件(别忘了启动时连接)
    2.启动时进入固件,选择CDROM项进入PE系统
    3.格式化主系统的C盘(请注意区分哪个才是主系统的C盘)
    4.向虚拟机里添加一个虚拟光盘,选择Windows10的ISO镜像(别忘了连接)
    5.打开 "Windows安装器"(英文名:winntsetup)

### winntsetup的配置

  **1.选择安装映像文件位置:**

    1.点击 选择
    2.可以通过下方的搜索框看见,他实际想让你找到并选择install.wim这个文件.
    3.点击Windows10ISO模拟出的虚拟光驱.(DVD驱动器)
    4.打开 sources 文件夹
    5.选择install.wim

![3e7d1be415f128654523b3d0167e5386.png](https://i.miji.bid/2024/02/29/3e7d1be415f128654523b3d0167e5386.png)

  **2.选择可引导驱动器的位置**

    1.点击向下的小箭头,选择被标记为绿色的分区(原理以后讲解)
    2.如果一旁的EFI PART旁边的圆点是绿色的,说明你的选择是正确的

  **3.选择安装驱动器的位置**

    1.点击向下的小箭头,通过列出的分区的大小选择你想把Windows安装在哪一个分区里

  **4.杂项**

    1.Windows版本选择Windows10专业版
    2.挂载安装驱动器为:C 的含义是你可以自定义系统盘盘符.你可以在这里随意选择自己的系统盘盘符
    3.点击右下角 安装
    4.下一页面,直接点击 确定

  **请等待一小段时间,winntsetup正在释放Windows10系统文件至所选系统分区.**

  **结束后,重启,进入系统开箱(OOBE)阶段.**

  **这次你可以看到OOBE的全过程.**

  **请断开虚拟机的网络连接,点击右下角的网络图标->断开连接**

    为了防止在OOBE阶段下载不必要的更新包和强制你登录微软账户,所以要断网.

![a9b30f3d585f64aa4499983457c6d00c.png](https://i.miji.bid/2024/02/29/a9b30f3d585f64aa4499983457c6d00c.png)
![9b4e10fafe603135cf2cb1a5a4b8a9a0.png](https://i.miji.bid/2024/02/29/9b4e10fafe603135cf2cb1a5a4b8a9a0.png)
![eda06fad9424d625198b22b8021c3fd7.png](https://i.miji.bid/2024/02/29/eda06fad9424d625198b22b8021c3fd7.png)
![7c47532b4943a5259b1ab2e7246237dd.png](https://i.miji.bid/2024/02/29/7c47532b4943a5259b1ab2e7246237dd.png)
![ad65f68069e95b851355f3c8d906d5fa.png](https://i.miji.bid/2024/02/29/ad65f68069e95b851355f3c8d906d5fa.png)
![57481e5352498600ad2e7f280d54014f.png](https://i.miji.bid/2024/02/29/57481e5352498600ad2e7f280d54014f.png)
![03739c096e1f4604286692227079baee.png](https://i.miji.bid/2024/02/29/03739c096e1f4604286692227079baee.png)
![fcd1c0d1a4f7a19f88a97f3c780a8e69.png](https://i.miji.bid/2024/02/29/fcd1c0d1a4f7a19f88a97f3c780a8e69.png)
![b6da8e8f291dcbe258f5b43fe94e3a7d.png](https://i.miji.bid/2024/02/29/b6da8e8f291dcbe258f5b43fe94e3a7d.png)
![a715c8da4d0b7dfa3c6d6cd5faf00d56.png](https://i.miji.bid/2024/02/29/a715c8da4d0b7dfa3c6d6cd5faf00d56.png)
![7472ce4c8321949e9d78eaa1364688ac.png](https://i.miji.bid/2024/02/29/7472ce4c8321949e9d78eaa1364688ac.png)
![eab26be6a1eef64a6bc50807738ca5d5.png](https://i.miji.bid/2024/02/29/eab26be6a1eef64a6bc50807738ca5d5.png)
![0cf54255b2a046dec3a77a4107187ad3.png](https://i.miji.bid/2024/02/29/0cf54255b2a046dec3a77a4107187ad3.png)