``` shell
先安装：
sudo apt-get install ntfs-config
再配置一下：
sudo ntfs-config
然后就会弹出来一个对话框，选择你需要挂载的分区，点应用，再选择“启用内部设备写支持”就搞定了。

手动设置ubuntu自动挂载Windows分区方法：

编辑/etc/fstab文件 $sudo gedit /etc/fstab 弹出geidt的文本编辑框，在文件尾部添加如下内容：

1.先用FDISK命令查看一下磁盘的UUID 
$sudo fdisk -l 
/dev/sda1 * 1 851 6835626 83 Linux 
/dev/sda2 852 4039 25607610 f W95 Ext'd (LBA) 
/dev/sda5 945 2135 9566676 7 HPFS/NTFS 
/dev/sda6 2136 4039 15293848+ 7 HPFS/NTFS 
2.NTFS分区添加如下内容，重启即可自动挂载NTFS分区了。 
/dev/sda6 /media/my ntfs-3g defaults,locale=zh_CN.UTF-8 0 0 
```
