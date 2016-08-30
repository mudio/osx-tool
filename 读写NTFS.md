## OSX读写NTFS格式磁盘

> 我不喜欢安装一些乱七八糟的软件，如果大家没有此类毛病可以使用工具`paragon`

##### :point_right: 方法1
```shell
$ diskutil info /Volumes/diskname | grep UUID
$ echo "UUID=上面获取的UUID none ntfs rw,auto,nobrowse" | sudo tee -a /etc/fstab
```
重新插入USB设备，键入`Command-Shift-G`,去往`/Volumes`目录就会发现可写的磁盘

##### :point_right: 方法2
```shell
#高危命令
sudo -s
cd /sbin
# 将系统自带的挂载程序改名
mv mount_ntfs mount_ntfs_orig
# 新建我们要的挂载脚本并编辑
vim mount_ntfs

    #!/bin/sh
    /sbin/mount_ntfs_orig -o rw,nobrowse "$@"

# 保存退出后改一下权限
chmod a+x mount_ntfs
# 都搞定了, 退出 root 身份
exit
```

重新插入USB设备，键入`Command-Shift-G`,去往`/Volumes`目录就会发现可写的磁盘
