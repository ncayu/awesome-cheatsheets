# Linux虚拟机磁盘扩容



### 1.查看磁盘信息

```bash
lsblk
```



### 2.查看待分区的磁盘的信息

```bash
fdisk -l
```



### 3.查看磁盘情况：打印可用空间

```bash
parted /dev/sda  print free
```



### 4.分配剩余的可用空间

将剩余的可用空间分配到 /dev/sda2，根据实际修改磁盘
resizepart 中的2 只的是第二个分区即：/dev/sda2
**100% 将所有的空闲空间分配给/dev/sda2,也可以用单位和百分比**

```bash
parted /dev/sda resizepart 2 100%
```



### 5.刷新物理卷

分区的空间修改了，也要刷新一下pv物理卷的大小，这样pv才能识别变动的空间

```bash
pvresize /dev/sda2
```



### 6.查看当前的可用空间

```bash
pvdisplay
```



```bash
vgs
```



### 7.查看LV Path



```bash
lvdisplay
```



### 8.扩展/root所在的lv



```bash
lvextend -L +100G /dev/centos/root    #扩展/root所在的lv，增加100G

xfs_growfs /dev/centos/root    #使/root文件扩展生效
```



### 9.查看磁盘大小

```bash
df -h
```

