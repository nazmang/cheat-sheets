Extend Volume Group capacity
```shell
vgextend centos /dev/sdc
```
Extend Logical Volume to maximum available size
```shell   
lvextend -l +100%FREE /dev/mapper/root
```
Extend Logical Volume by 2G 
```shell
lvextend -L +2G /dev/mapper/root
```
Resize FS on Logical Volume 
```shell
resize2fs /dev/mapper/root
```
Resize XFS
```shell
xfs_growfs /mnt/point
```