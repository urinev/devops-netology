1. 
2. права доступа и владелец одинаковы на всех hardlink т.к. указывают на одну inodes
3. Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:
```
vagrant@vagrant:~$ lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0                       7:0    0 55.4M  1 loop /snap/core18/2128
loop1                       7:1    0 70.3M  1 loop /snap/lxd/21029
loop2                       7:2    0 32.3M  1 loop /snap/snapd/12704
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0    1G  0 part /boot
└─sda3                      8:3    0   63G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm  /
sdb                         8:16   0  2.5G  0 disk
sdc                         8:32   0  2.5G  0 disk
```
4. Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.
```
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
└─sdb2                      8:18   0  511M  0 part
sdc                         8:32   0  2.5G  0 disk
```
5. Используя sfdisk, перенесите данную таблицу разделов на второй диск.
```
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
└─sdb2                      8:18   0  511M  0 part
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
└─sdc2                      8:34   0  511M  0 part
```
6. Соберите mdadm RAID1 на паре разделов 2 Гб.
```
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdb2                      8:18   0  511M  0 part
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdc2                      8:34   0  511M  0 part
vagrant@vagrant:~$ cat /proc/mdstat
Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]
md0 : active raid1 sdc1[1] sdb1[0]
      2094080 blocks super 1.2 [2/2] [UU]
```
7. Соберите mdadm RAID0 на второй паре маленьких разделов.
```
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdb2                      8:18   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdc2                      8:34   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
vagrant@vagrant:~$ cat /proc/mdstat
Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]
md1 : active raid0 sdc2[1] sdb2[0]
      1042432 blocks super 1.2 512k chunks

md0 : active raid1 sdc1[1] sdb1[0]
      2094080 blocks super 1.2 [2/2] [UU]
```
8. Создайте 2 независимых PV на получившихся md-устройствах.
```
root@vagrant:~# pvdisplay
  --- Physical volume ---
  PV Name               /dev/sda3
  VG Name               ubuntu-vg
  PV Size               <63.00 GiB / not usable 0
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              16127
  Free PE               8063
  Allocated PE          8064
  PV UUID               sDUvKe-EtCc-gKuY-ZXTD-1B1d-eh9Q-XldxLf

  "/dev/md0" is a new physical volume of "<2.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/md0
  VG Name
  PV Size               <2.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               i7SbKy-Dxk2-E0xd-xVLi-mso1-zgom-Xcl5Yp

  "/dev/md1" is a new physical volume of "1018.00 MiB"
  --- NEW Physical volume ---
  PV Name               /dev/md1
  VG Name
  PV Size               1018.00 MiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               1TdX8V-F5kQ-3Np3-5sr0-ZEAa-i1Im-CwvWnY
```
9. Создайте общую volume-group на этих двух PV.
```
 --- Volume group ---
  VG Name               vg01
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               <2.99 GiB
  PE Size               4.00 MiB
  Total PE              765
  Alloc PE / Size       0 / 0
  Free  PE / Size       765 / <2.99 GiB
  VG UUID               fifby8-PXL7-6eaF-dGfN-emCx-cMrZ-L3y0QC
```
10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.
```root@vagrant:~# lvcreate -L 100M vg01 /dev/md1
  Logical volume "lvol0" created.
root@vagrant:~# lvs
  LV        VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  ubuntu-lv ubuntu-vg -wi-ao----  31.50g
  lvol0     vg01      -wi-a----- 100.00m
root@vagrant:~# vgs
  VG        #PV #LV #SN Attr   VSize   VFree
  ubuntu-vg   1   1   0 wz--n- <63.00g <31.50g
  vg01        2   1   0 wz--n-  <2.99g   2.89g
```
11. Создайте ```mkfs.ext4``` ФС на получившемся LV.
```
root@vagrant:~# mkfs.ext4 /dev/vg01/lvol0
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 25600 4k blocks and 25600 inodes

Allocating group tables: done
Writing inode tables: done
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done
```
12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.
13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.
14. 
```
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md126                   9:126  0    2G  0 raid1
└─sdb2                      8:18   0  511M  0 part
  └─md127                   9:127  0 1018M  0 raid0
    └─vg01-lvol0          253:1    0  100M  0 lvm   /tmp/new
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md126                   9:126  0    2G  0 raid1
└─sdc2                      8:34   0  511M  0 part
  └─md127                   9:127  0 1018M  0 raid0
    └─vg01-lvol0          253:1    0  100M  0 lvm   /tmp/new
```
15. OK
16. 
```
root@vagrant:~# pvmove /dev/md127
  /dev/md127: Moved: 60.00%
  /dev/md127: Moved: 100.00%
root@vagrant:~# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 55.4M  1 loop  /snap/core18/2128
loop1                       7:1    0 55.5M  1 loop  /snap/core18/2284
loop2                       7:2    0 61.9M  1 loop  /snap/core20/1328
loop3                       7:3    0 70.3M  1 loop  /snap/lxd/21029
loop4                       7:4    0 67.2M  1 loop  /snap/lxd/21835
loop5                       7:5    0 32.3M  1 loop  /snap/snapd/12704
loop6                       7:6    0 43.4M  1 loop  /snap/snapd/14549
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0    1G  0 part  /boot
└─sda3                      8:3    0   63G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md126                   9:126  0    2G  0 raid1
│   └─vg01-lvol0          253:1    0  100M  0 lvm   /tmp/new
└─sdb2                      8:18   0  511M  0 part
  └─md127                   9:127  0 1018M  0 raid0
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md126                   9:126  0    2G  0 raid1
│   └─vg01-lvol0          253:1    0  100M  0 lvm   /tmp/new
└─sdc2                      8:34   0  511M  0 part
  └─md127                   9:127  0 1018M  0 raid0
```
17. Сделайте --fail на устройство в вашем RAID1 md.
18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.
```
[ 2057.956236] md/raid1:md126: Disk failure on sdc1, disabling device.
               md/raid1:md126: Operation continuing on 1 devices.
```
```
root@vagrant:~# cat /proc/mdstat
Personalities : [raid0] [raid1] [linear] [multipath] [raid6] [raid5] [raid4] [raid10]
md126 : active raid1 sdb1[0] sdc1[1](F)
      2094080 blocks super 1.2 [2/1] [U_]

md127 : active raid0 sdb2[0] sdc2[1]
      1042432 blocks super 1.2 512k chunks
```
19. Тест ОК