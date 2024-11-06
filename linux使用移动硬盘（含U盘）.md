## 查看磁盘设备

```cmd
fdisk -l
```

## 挂载usb存储设备

将sda1挂载到 /usbdisk 目录

-t 文件系统类型，常用的有 nefs ext4 fast32 exfat

```cmd
mkdir /usbdisk
mount -t ntfs /dev/sda1 /usbdisk
```

## 卸载usb存储设备

```cmd
umount /usbdisk
```

如果卸载出错 umount: /xxx: device is busy. ， 使用fuser命令查看占用磁盘的进程.

安装fuser命令:

```cmd
yum install psmisc 
```

查看占用信息：

```cmd
fuser -mv /usbdisk
```

##  使用udisksctl安全移除移动硬盘

使用umount命令卸载后，发现移动还处于通电状态，可使用udisksctl断开设备。

确认你的系统上已经安装好了 udisks2 这个包

```cmd
apt install udisks2
```

sdb是我的u盘设备，sdb1是u盘上的文件系统对应的设备。
```cmd
udisksctl unmount -b /dev/sdb1
udisksctl power-off -b /dev/sdb
```