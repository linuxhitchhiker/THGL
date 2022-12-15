---
title: 在虚拟机中安装 Linux
comments: true
---

# 在虚拟机中安装 Linux

!!! note
    本文只是虚拟化软件的简单介绍和使用，具体更为细节的内容请自行翻阅相关文档。同时，本文假定读者当前使用的操作系统是 Windows。

## 虚拟机与物理机

在不熟悉 Linux 基本知识的情况下，直接在物理机上安装 Linux 可能会导致数据丢失，或者是硬件损坏。

虚拟机作为一个让你在不破坏当前系统结构的情况下，能够获得最接近原生 Linux 环境体验的工具。非常适合 Linux 初学者安全地练习如何安装和使用 Linux。

!!! attention
    在物理机上安装 Linux 并非是必须的步骤，不安装到物理机上可以省去大量的迁移工作，但安装到物理机上会让系统具备更强的性能和更多的功能。  
    本文所描述的虚拟化软件都有 Linux 版。

本文主要描述如何使用两种流行的虚拟化软件：

- [Virtualbox](https://www.virtualbox.org/)：自由，开源，免费。
- [VMware Workstation](https://www.vmware.com/products/workstation-pro.html)：历史悠久的企业级虚拟化产品，闭源，收费。
- [VMWare Workstation Player](https://www.vmware.com/cn/products/workstation-player/workstation-player-evaluation.html)：VMware Workstation 的免费版本，不支持多个虚拟机同时运行。

## Virtualbox

### 安装 Virtualbox

打开 [Download VirtualBox](https://www.virtualbox.org/wiki/Downloads) 页面，点击 `Windows hosts` 下载适用于 Windows 系统的 virtualbox 安装包。然后在此页面找到并下载 `VirtualBox Oracle VM VirtualBox Extension Pack`。

打开 virtualbox 安装程序，依照提示完成安装。完成安装后，即可启动管理器：

![begain](./assets/vm/vbox-begain.png)

### 安装扩展包

`VirtualBox Oracle VM VirtualBox Extension Pack` 扩展包主要提供了 USB 驱动和 3D 加速驱动等因版权无法自由分发的内容。

要安装扩展包，请先打开 VirtualBox，点击左侧**工具**栏上的选项按钮，切换到**扩展**页面，然后再点击上方的 **install** 安装你刚刚下载保持的扩展包文件，然后你就会看到扩展包的使用许可协议，滚动到底端，然后点击**我同意**，即可安装扩展包：

![extensions](./assets/vm/vm-install-expak.gif)

### 新建虚拟机

!!! note
    将鼠标光标停留在某个选项上会显示该选项的简易使用说明。  
    注意，请将虚拟机安装在固态硬盘分区中以提高虚拟机的性能。  

打开 VirtualBox ，点击**新建** ，输入虚拟机的名称（VirtualBox 会根据你输入的名称快速筛选虚拟机的版本和类型），点击下一步:

![name](./assets/vm/vbox-vm1.png)

内存大小建议最小值为 **2048MB**，具体的最低配置要求详见你所安装的系统版本的[最低配置要求](./pre-install.md)。点击下一步:

![ram](assets/vm/vbox-vm2.png)

- CPU 核心数量可以拉到一半的位置，同时可以根据使用情况考虑是否启用 EFI。
- 并不建议将指针拖动到红色区域，这可能会对主机的正常运行造成影响。

在选择默认的**现在创建虚拟硬盘**，虚拟硬盘文件类型默认为 VDI。点击下一步，选择**动态分配**，大小建议 **20GB** 或更高值，具体的最低配置详见你所安装的系统版本的[最低配置要求](./pre-install.md)。点击下一步完成创建：

![vbox](./assets/vm/vbox-vm3.png)

![finish](./assets/vm/vbox-vm4.png)

### 配置虚拟机

在启动安装前，你还需要进行一些额外的配置。

点击虚拟机详情页中的**设置** ，在**常规**页面中，点击**高级**，然后为虚拟机启用剪贴板共享和文件拖放：

![reg](./assets/vm/vbox-conf-reg.png)

再点击**系统**；在**主板**页面，你可以点击勾选**启用 EFI**（某些系统需要用户启用 UEFI 支持）或安全启动支持。在**处理器**页面，你可以更改虚拟机使用的**CPU 核心数**：

![sys](./assets/vm/vbox-conf-sys.png)

!!! note
     给虚拟机分配过多的资源会导致宿主机卡顿。你可以修改该页面中的启动顺序来改变虚拟机启动时引导设备的顺序。虚拟机系统安装完成后 VirtualBox 需要用户手动移除虚拟盘片。

注意，你必须在**显示**页面中，将**显存大小**拉满，否则你很有可能会遇到显存耗尽的问题。同时可以考虑是否为虚拟机启用 3D 图形加速（具体取决于你的硬件性能）。 

![display](./assets/vm/vbox-conf-display.png)

!!! note
    如果你遇到无法调节虚拟机屏幕分辨率的问题，你可以在关闭虚拟机后，更换虚拟机使用的虚拟显卡或者关闭 3D 图形加速。

在**存储**中，点击**没有盘片** ，再点击**分配光驱**右侧的光碟小图标，再点击**选择虚拟盘**，找到并选中你准备好的 ISO 镜像文件。 

![iso](./assets/vm/vbox-conf-iso.png)

在 **USB 设备**中，点击右侧的**添加一个 USB 筛选器**，勾选你插入宿主机的 USB 设备。

!!! note
    该步骤为可选操作。一旦启动虚拟机，你插入宿主机的 USB 设备就会自动被重定向到虚拟机中，并且在虚拟机关机前，你都不能在宿主机访问该 USB 设备。

在**共享文件夹**中，点击右侧的**添加共享文件夹** ，点击**共享文件夹路径**右侧的倒三角符号，点击**其他**，选择一个文件夹用于共享文件:

![share](./assets/vm/vbox-conf-share.png)

!!! note
    - 该步骤为可选操作。共享文件夹可以实现主宿机间文件的实时交流。请勿将虚拟机的任何程序或者程序所使用的文件夹安装或存放到共享文件夹中。  
    - 建议勾选**自动挂载**以便于虚拟机在开机后自动发现共享文件夹。

如果你在配置虚拟机的时候，没有指定使用的系统镜像文件。虚拟机在启动的时候会提醒你选择一个镜像文件。点击提示页面右侧的**选择一个虚拟光盘文件** ，再点击**注册**，找到并选中你下载的光盘文件，点击你新添加的镜像文件，再点击**选择**，确定无误后启动虚拟机。

### 注册 ISO 文件

打开 Virtualbox，点击**工具**，然后切换到**介质**页面，此时你可以看到 virtualbox 已经使用的虚拟机磁盘和镜像文件：

![iso-list](./assets/vm/vbox-iso-man.gif)

你可以在虚拟镜像文件页面中将已下载好的 ISO 文件都导入到此列表中，方便后续新建虚拟机的时候可以直接使用。

### 快照

快照是一个非常有用的功能，它可以将你的虚拟机恢复到指定的状态，特别适合于测试环境。

点击要生成快照的虚拟机，然后切换到**备份**页面，新建一个快照即可。当你需要使用的时候，再点击恢复备份：

![snapshot](./assets/vm/vbox-snapshot.gif)

### 安装增强功能

启动虚拟机后，Virtualbox 会自动捕获用户的鼠标光标，你可以按 `右 Ctrl` 取消捕获。按 `右 Ctrl + F` 进入全屏。

你可以通过命令行安装 virtualbox 的增强功能包：

```
sudo zypper in virtualbox-guest-tools    #适用于 openSUSE
sudo dnf in virtualbox-guest-tools       #适用于 Fedora
```

然后将你的用户添加至 `vboxsf` 用户组：

```
sudo usermod -aG vboxsf $USER
```

重新登录系统即可看到你之前设置好的共享文件夹（该共享文件夹一般位于 `/media` 目录之下，如果你没有看到自动挂载的文件夹，你需要手动将共享文件夹固定到文件浏览器的侧边栏之中）。

### 断开网络连接和虚拟盘

要断开网络连接，只需要点击底部图标，取消勾选网络连接即可：

![net](./assets/vm/vbox-net.png)

在系统安装完成后，virtualbox 并不会自动弹出 ISO 文件，你可以手动点击底栏图标，移除虚拟盘即可：

![disk](./assets/vm/vbox-disk.png)

### 其他

- [Virtualbox Documentation](https://www.virtualbox.org/wiki/Documentation)

### 增强虚拟机的图形性能

要增强虚拟机的性能，主要有以下几个办法：

1. 提高虚拟机可使用的 CPU 核心数；
2. 提高虚拟机可使用的 RAM；
3. 将虚拟机放置在宿主机的固态硬盘上以提高读写性能；
4. 使用独立显卡运行 virtualbox（如果出现撕裂问题，可以在设置中关闭 3D 图形加速）。

!!! note "注意"
    以上的建议也适用于 VMware Workstation。

----

## VMWare Workstation

### 安装

打开 [VMware Workstation Pro](https://www.vmware.com/cn/products/workstation-pro.html)，点击 **试用 Workstation 16 Pro** 进入下载页面。下载完成后，打开安装程序，依照引导提示完成安装。有关 VMware Workstation Pro 的商业授权使用详见[此处](https://store-us.vmware.com/vmware-workstation-16-pro-5424176500.html)。

如果你需要免费的 VMware 虚拟化产品，你可以选择使用 [VMware Workstation Player](https://www.vmware.com/cn/products/workstation-player.html)，此站点的 **比较** 页面分析了 Workstation Player 和 Workstation Pro 的差异，以及你适合哪一种产品。

### 新建虚拟机

在安装好 VMWare Workstation 后，打开 VMware，点击左上方**文件**，选择**新建虚拟机**。

![Begain](./assets/vm/vm-begain.png)

在弹出的新建虚拟机向导中，点击**下一步**，然后选择**安装程序光盘映像文件**，打开你下载好的 Linux ISO 文件。点击**下一步**：

![new01](./assets/vm/vm-new01.png)

在“命名虚拟机”中指定你的虚拟机的名称和虚拟机保存的位置。点击**下一步**：

![new02](./assets/vm/vm-new02.png)

在此页面，你可以指定虚拟机的**最大磁盘大小**，你可以使用默认值，或者指定更大的容量。完成后点击**下一步**：

![new03](./assets/vm/vm-new03.png)

在“已准备好创建虚拟机”页面中，检查虚拟机硬件配置是否合乎要求。你可以点击**自定义硬件**，自行调整配置（VMware 默认的配置一般不适合使用 Linux 桌面环境）。

![new04](./assets/vm/vm-new04.png)

![new05](./assets/vm/vm-new05.png)

完成后点击**完成**启动虚拟机。`Ctrl + Alt` 快捷键组合可以让 VMware 停止捕获你的鼠标。如果你勾选了**创建后开启此虚拟机**，则 VMware 会自动启动虚拟机。

### 安装 open-vm-tools

一般而言，Fedora 和 openSUSE 都默认预置了 `open-vm-tools`，你也可以手动安装该扩展包。

```
sudo zypper in open-vm-tools  #适用于 openSUSE
sudo dnf in open-vm-tools     #适用于 Fedora
```

安装完成后，重启系统。

### 创建共享文件夹

在关闭虚拟机后，点击**编辑虚拟机设置**，再点击**选项**，找到并点击**共享文件夹**，将设置调整为**总是启用**，然后在下方点击**添加**，选择你所要用于主机-虚拟机共享的文件夹：

![share](./assets/vm/vm-sharefolder.png)

确认并保存后，启动系统，然后在终端中键入以下命令启用共享文件夹。

1. 使用 `mkdir` 命令创建一个名为 Share 的文件夹：  
    ```
    mkdir Share
    ```

2. 使用 `nano` 命令编辑 `/etc/fuse.conf`：  
    ```
    sudo nano /etc/fuse.conf
    ```
    然后使用方向键移动光标，删掉 `user_allow_other` 前的 `#`:
    ![nano](./assets/vm/vm-nano.png)  
    按下 `Ctrl + O` 保存更改，再按下 `Ctrl + X` 退出 `nano` 编辑器；

3. 运行下列命令挂载共享文件夹：
    ```
    /usr/bin/vmhgfs-fuse .host:/ ~/Share -o subtype=vmhgfs-fuse,allow_other
    ```
    ![end](./assets/vm/vm-share-end.png)

### UEFI 和 BIOS

如果你需要将虚拟机的启动模式设置为 UEFI，而非 VMware 默认的 BIOS。则你应该在新建虚拟机的时候，在第一步选择**稍后安装系统**，然后将**客户机操作系统**的版本设置为**其他 Linux 5.x 内核 64 位**。然后再在虚拟机的设置页面的 CD/DVD (IDE) 选项中添加用于安装系统的 ISO 文件。

### 官方文档

有关 Workstation 更多的使用指南详见：

- [VMware Workstation Pro Documentation](https://docs.vmware.com/en/VMware-Workstation-Pro/index.html)
- [VMware Workstation Player Documentation](https://docs.vmware.com/en/VMware-Workstation-Player/index.html)
- [VMware Fusion Documentation](https://docs.vmware.com/en/VMware-Fusion/index.html)