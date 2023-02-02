# Archlinux 安装随笔

#### 进入archlinux引导界面

- 在vm中安装arch，与其他系统并无不同，唯一不同点是要选择UEFI引导固件
- win arch双系统
  - 关闭win11快速启动
  - windows磁盘分区
    - 压缩空间就可以，让arch空间在win上未分配
  - 制作启动U盘
    - u盘软件 https://rufus.ie/
  - BIOS设置
    - 开机按某个键（F2）进入BIOS
    - Secure Boot 设置为Disabled
    - USB UEFI BIOS SUPPORT 设置为Enabled
    - UEFI/Legacy Boot 设为UEfl Only
  - 关机，插入U盘，开机，选择USE HDD
- 选择 Arch Linux install medium 并按下Enter

----

#### 验证引导模式

`ls /sys/firmware/efi/efivars` 

- 如果命令结果显示了目录且没有报告错误，则系统以 UEFI 模式引导。 如果目录不存在，则系统可能以 [BIOS](https://zh.wikipedia.org/wiki/BIOS) 模式 (或 [CSM](https://en.wikipedia.org/wiki/Compatibility_Support_Module) 模式) 引导。如果系统未以您想要的模式引导启动，请参考您的主板说明书。

----

#### 命令介绍

`nomodeset  video=800x450` 调整屏幕大小

`setfont /usr/share/kbd/consolefonts/...` 字体文件

`Ctrl + L` 清屏

----

#### 网络链接

##### 网线

- 正常来说，只要插上一个已经联网的路由器分出的网线（DHCP），直接就能联网。

  可以等待几秒等网络建立连接后再进行下一步测试网络的操作。

  若笔记本没有网线接口请使用带网线接口的扩展坞

##### wifi

- `ip link` 查看互联网设备
  - `ip link set wlan0 up` 启动互联网设备，waln0是查到的设备
- `iwlist wlan0 scan` 扫描网络
  - `iwlist wlan0 scan | grep ESSID` grep搜索所有的ESSID的那一行
- `wpa_passphrase wifi名 > internet.conf ` 生成网络配置
  - `wpa_passphrase -c internet.conf -i wlan0 &` 连接网络
- `dhcpcd &` 动态分配ip地址
- `ping baidu.com` 检查网络

#### 更正系统时间

`timedatectl set-ntp true` 

----

#### 分区

 `fdisk -l` 查看存储设备

`fdisk /dev/存储设备` 进入设备并且分区

- `g` 创建新的分区列表
- `w` 写入
- `n` 创建第一个分区
  - first sector 默认是从还未被选择的地方开始
  - last sector 大小


`mkfs.fat -F32 "boot系统引导地方"` 系统引导区格式处理

`mkfs.ext4 "/系统主分区"`  系统主分区格式处理

`mkswap /swap区` 变成swap区

- `swapon /swap区` 挂载区

##### 分区知识点

###### UEFI  

- /mnt/boot 启动磁盘，当前空间与其他系统共享
- /mnt 系统主分区
- swap 系统虚拟内存，休眠时存储空间

----

#### pacman文件配置

`vim /etc/pacman.conf` 打开文件

- `Color` 行，安装输出的时候加颜色
- `Include = /etc/pacman.d/mirrorlist` mirrorlist就是源文件列表

`vim 宏录制` 

- `qa` 开启宏录制,一共有a、b、c三个vim寄存器，q跟哪个待会就用哪个
- `/^\n` 空行
- `q` 关闭宏录制

----

#### pacstrap 安装脚本

- /mnt 系统文件夹，在这里安装系统，一个设备，系统的硬盘

`mount 主分区位置 /mnt` 将系统挂载到/mnt目录下，一定要先挂载这个

`mount --mkdir /dev/efi_system_partition（EFI 系统分区） /mnt/boot` 挂载这个boot

`pacstrap -K /mnt base linux linux-firmware` 安装最基础的arch系统

- base arch最基础的系统
- linux 系统内核 linux-firmware 系统框架

`genfstab -U /mnt >> /mnt/etc/fstab` 生成fstab文件

- `mount -a` 检查挂在是否成功，如果没有报错，说明fstab文件没有问题

----

#### 进入安装好的系统

`arch-chroot /mnt`  进入安装好的系统

##### 更正系统时间

- `ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime` 
- `hwclock --systohc` 时间同步

`exit` 退出

##### 生成locales UTF-8

`vim /mnt/etc/locale.gen`  本地化

- `/en_US` 找到这一行，取消注释，保存退出

`arch-chroot /mnt` 生成本地化的东西

#### 配置语言

`vim /mnt/etc/local.conf` 

- LANG=en_US.UTF-8

#### 配置网络名字

`vim /mnt/etc/hostname` 

- sj 放在开头就行了

#### 配置端口

`vim /mnt/etc/hosts`  

接上

```
127.0.0.1		localhost
::1				localhost
127.0.1.1		sj.localdomain sj
```

----

#### 进入系统

`arch-chroot /mnt`

`passwd` 创建密码

安装一些软件

- `pacman -S grub efibootmgr intel-ucode os-prober`

生成grub配置

- `mkdir /boot/grub`

- `grub-mkconfig > /boot/grub/grub.cfg`

确定系统架构

- `uname -m` 电脑一般是x86_64

生成grub

- `grub-install --target=x86_64-efi --efi-directory=/boot`
  - `grub-install —target=x86_64-efi —bootloader-id=GRUB —efi-directory=/boot/efi —nvram —remov` 出现错误，可以这样 

在系统里面安装一些软件

`pacman -S neovim vi zsh wpa_supplicant dhcpcd` 

- wpa_supplicant 互联网工具
- dhcpcd 动态分配ip地址

`exit`

`killall wpa_supplicant dhcpcd`

`reboot` 重启

----

#### 后记

`systemctl enable --now dhcpcd` 网络

`ping www.baidu.com` 记得加www
