# Universal_USB_Bootdisk

## 简介
一个通用的USB启动盘,支持从
* UEFI模式启动
    * WinPE
    * Win10 ISO
    * Ubuntu, Arch Linux等发行版的Live CD
* BIOS模式启动
    * WinPE
    * Win7 ISO
    * Ubuntu, Arch Linux等发行版的Live CD

## 制作方法

### 材料准备
16G以上U盘一个, Ubuntu 14.04 LTS

### U盘格式化以及分区
* 删除U盘所有分区, 为U盘建立MBR分区表.
* 划分100MB的FAT32格式分区并设置标记为boot(用于存放grub2和grub4dos). 
* 划分不少于300MB的FAT32格式分区(用于存放解压后的PE ISO).
* 划分不少于6GB的NTFS分区(用于存放解压后的Win10 ISO).
* 剩余空间划分为NTFS分区, 根目录建立iso文件夹存放镜像文件.

### 安装GRUB2和GRUB4DOS
```shell
sudo mount /dev/sdb1 /mnt # U盘第一个分区对应/dev/sdb1, 通过sudo parted -l 确认
sudo grub-install --target=i386-pc --recheck --boot-directory=/mnt/boot /dev/sdb
sudo grub-install --target x86_64-efi --efi-directory /mnt --boot-directory=/mnt/boot --removable
```
下载GRUB4DOS并解压到第一个分区的根目录.

### 解包, 拷贝镜像文件
见"文件放置说明".

## 文件放置说明

## To Be Done...
