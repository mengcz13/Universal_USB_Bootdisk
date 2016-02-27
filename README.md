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
```Shell
sudo mount /dev/sdb1 /mnt # U盘第一个分区对应/dev/sdb1, 通过sudo parted -l 确认
sudo grub-install --target=i386-pc --recheck --boot-directory=/mnt/boot /dev/sdb
sudo grub-install --target x86_64-efi --efi-directory /mnt --boot-directory=/mnt/boot --removable
```
下载GRUB4DOS并解压到第一个分区的根目录.

### 生成GRUB2的配置文件grub.cfg
GRUB2已经采用grub-mkconfig程序自动生成配置文件. 该程序用到`/etc/default/grub`配置文件和`/etc/grub.d/`目录下的生成模板.

**警告: 操作前注意备份上述文件**

* 备份`/etc/default/grub`配置文件和`/etc/grub.d/`目录.
* 删除系统中对应文件, 使用git仓库中的对应文件替代系统文件.
* 生成grub.cfg
```Shell
sudo grub-mkconfig -o /path/you/want/grub.cfg
```
* 使用生成的文件替换U盘首分区`/boot/grub/grub.cfg`(不存在则创建)

### 解包, 拷贝镜像文件
见"文件放置说明".

## 文件放置说明

详细的文件路径请参考`grub.d/40_custom`.

### WinPE
ISO文件解压后置于第二分区根目录.

### Win10
ISO文件解压后置于第三分区根目录. 此分区同样可放置需要UEFI引导的其他Windows 安装盘文件.

### Win7以及其他需要BIOS引导的Windows安装盘(仅支持BIOS引导)
将ISO文件**直接**置于第四分区iso文件夹内.

### Linux各发行版
将ISO文件**直接**置于第四分区iso文件夹内.

## 背景图片
`/mnt/boot/grub/bg.jpg`, 新增或更改后需要重新生成配置文件.

## To Be Fixed...
* UEFI引导方式下一个分区只能有一个系统的安装文件且必须位于根目录.
* Arch Linux需要将`/arch/x86_64/airootfs.sfs`提取到根目录. 否则安装程序找不到.
* 更多Linux发行版待添加...
