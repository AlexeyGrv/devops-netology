1. Узнайте о sparse (разряженных) файлах.

Разреженный файл: пустые байты не нужно сохранять, но они могут быть представлены метаданными. Таким образом, сохраняется только сама информация и ситуация, в которой находятся пробелы.
В вычислениях разреженный файл — это тип компьютерного файла, который пытается использовать пространство файловой системы более эффективно, когда блоки (пространство), выделенные для файлов, в основном пусты.

Это достигается путем записи краткой информации ( метаданных ), представляющей пустые блоки на диске, а не фактическое «пустое» пространство, которое составляет блок, что позволяет использовать меньше места на диске. Полный размер блока записывается на диск как фактический размер только тогда, когда блок содержит «реальные данные» (не пустые). При чтении разреженных файлов файловая система прозрачно преобразует метаданные, представляющие пустые блоки, в блоки, заполненные нулевым байтом, во время выполнения, поэтому приложение не знает об этом преобразовании.

Большинство современных файловых систем поддерживают разреженные файлы, включая большинство вариантов Unix и NTFS. Разреженные файлы обычно используются для образов дисков , копирования баз данных , файлов журналов и в научных приложениях.

Преимущества
Преимущество разреженных файлов в том, что хранилище выделяется только тогда, когда оно действительно необходимо: экономится место на диске, и можно создавать большие файлы, даже если в файловой системе недостаточно свободного места.

Недостатки
Некоторые из основных недостатков:

Файлы могут быть фрагментированы, поскольку пространство в файловой системе выделяется по мере заполнения дыр, и они не обязательно являются смежными.
Информация о свободном месте в файловой системе может ввести в заблуждение, учитывая видимый размер разрозненных файлов.
Нехватка свободного места в файловых системах, содержащих разреженные файлы, может привести к нежелательным последствиям.

2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?

Файл размещен в определенном месте жесткого диска. На это место могут ссылаться несколько ссылок из файловой системы. Каждая из ссылок - это отдельный файл, но ведут они к одному участку жесткого диска. Файл можно перемещать между каталогами, и все ссылки останутся рабочими, поскольку для них неважно имя. 
Рассмотрим особенности:
Работают только в пределах одной файловой системы;
Нельзя ссылаться на каталоги;
Имеют ту же информацию inode и набор разрешений что и у исходного файла;
Разрешения на ссылку изменятся при изменении разрешений файла;
Можно перемещать и переименовывать и даже удалять файл без вреда ссылке.

vagrant@vagrant:/tmp/new$ ls -l
total 21760
drwx------ 2 root root    16384 Apr 10 19:02 lost+found
-rw-r--r-- 1 root root 22262449 Apr 10 18:57 test.gz
vagrant@vagrant:/tmp/new$ sudo ln test.gz test_link
vagrant@vagrant:/tmp/new$ ls -l
total 43504
drwx------ 2 root root    16384 Apr 10 19:02 lost+found
-rw-r--r-- 2 root root 22262449 Apr 10 18:57 test.gz
-rw-r--r-- 2 root root 22262449 Apr 10 18:57 test_link
vagrant@vagrant:/tmp/new$ sudo chmod 0755 test.gz 
vagrant@vagrant:/tmp/new$ ls -l
total 43504
drwx------ 2 root root    16384 Apr 10 19:02 lost+found
-rwxr-xr-x 2 root root 22262449 Apr 10 18:57 test.gz
-rwxr-xr-x 2 root root 22262449 Apr 10 18:57 test_link



3. Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:

sudo vim Vagrantfile 

Vagrant.configure("2") do |config|
 	config.vm.box = "bento/ubuntu-20.04"
 	config.vm.network "forwarded_port", guest: 19999, host: 19998
 	config.vm.provider :virtualbox do |vb|
    lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"
    lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"
    vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]
    vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]
  end
end

4. Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.

vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk 
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part 
└─sda5                 8:5    0 63.5G  0 part 
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk 
sdc                    8:32   0  2.5G  0 disk 

vagrant@vagrant:~$ sudo fdisk /dev/sdb

Welcome to fdisk (util-linux 2.34).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0xf8ead2db.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-5242879, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2G

Created a new partition 1 of type 'Linux' and of size 2 GiB.

Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p): 

Using default response p.
Partition number (2-4, default 2): 
First sector (4196352-5242879, default 4196352): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242879, default 5242879): 

Created a new partition 2 of type 'Linux' and of size 511 MiB.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk 
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part 
└─sda5                 8:5    0 63.5G  0 part 
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk 
├─sdb1                 8:17   0    2G  0 part 
└─sdb2                 8:18   0  511M  0 part 
sdc                    8:32   0  2.5G  0 disk 

5. Используя sfdisk, перенесите данную таблицу разделов на второй диск.

vagrant@vagrant:~$ sudo sfdisk -d /dev/sdb|sudo sfdisk --force /dev/sdc
Checking that no-one is using this disk right now ... OK

Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Created a new DOS disklabel with disk identifier 0xf8ead2db.
/dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
/dev/sdc2: Created a new partition 2 of type 'Linux' and of size 511 MiB.
/dev/sdc3: Done.

New situation:
Disklabel type: dos
Disk identifier: 0xf8ead2db

Device     Boot   Start     End Sectors  Size Id Type
/dev/sdc1          2048 4196351 4194304    2G 83 Linux
/dev/sdc2       4196352 5242879 1046528  511M 83 Linux

The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk 
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part 
└─sda5                 8:5    0 63.5G  0 part 
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk 
├─sdb1                 8:17   0    2G  0 part 
└─sdb2                 8:18   0  511M  0 part 
sdc                    8:32   0  2.5G  0 disk 
├─sdc1                 8:33   0    2G  0 part 
└─sdc2                 8:34   0  511M  0 part 

6. Соберите mdadm RAID1 на паре разделов 2 Гб.

vagrant@vagrant:~$ sudo mdadm --create --verbose /dev/md0 -l 1 -n 2 /dev/sd{b1,c1}
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
mdadm: size set to 2094080K
Continue creating array? 
Continue creating array? (y/n) y 
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk  
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part  
└─sda5                 8:5    0 63.5G  0 part  
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk  
├─sdb1                 8:17   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1 
└─sdb2                 8:18   0  511M  0 part  
sdc                    8:32   0  2.5G  0 disk  
├─sdc1                 8:33   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1 
└─sdc2                 8:34   0  511M  0 part  

7. Соберите mdadm RAID0 на второй паре маленьких разделов.

vagrant@vagrant:~$ sudo mdadm --create --verbose /dev/md1 -l 0 -n 2 /dev/sd{b2,c2}
mdadm: chunk size defaults to 512K
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md1 started.
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk  
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part  
└─sda5                 8:5    0 63.5G  0 part  
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk  
├─sdb1                 8:17   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1 
└─sdb2                 8:18   0  511M  0 part  
  └─md1                9:1    0 1018M  0 raid0 
sdc                    8:32   0  2.5G  0 disk  
├─sdc1                 8:33   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1 
└─sdc2                 8:34   0  511M  0 part  
  └─md1                9:1    0 1018M  0 raid0 

8. Создайте 2 независимых PV на получившихся md-устройствах.

sudo pvcreate /dev/md0 /dev/md1
  Physical volume "/dev/md0" successfully created.
  Physical volume "/dev/md1" successfully created.

sudo pvscan
  PV /dev/sda5   VG vgvagrant       lvm2 [<63.50 GiB / 0    free]
  PV /dev/md0                       lvm2 [<2.00 GiB]
  PV /dev/md1                       lvm2 [1018.00 MiB]
  Total: 3 [<66.49 GiB] / in use: 1 [<63.50 GiB] / in no VG: 2 [2.99 GiB]

9. Создайте общую volume-group на этих двух PV.

vagrant@vagrant:~$ sudo vgcreate v_g /dev/md1 /dev/md0
  Volume group "v_g" successfully created

vagrant@vagrant:~$ sudo pvdisplay
  --- Physical volume ---
  PV Name               /dev/sda5
  VG Name               vgvagrant
  PV Size               <63.50 GiB / not usable 0   
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              16255
  Free PE               0
  Allocated PE          16255
  PV UUID               Mx3LcA-uMnN-h9yB-gC2w-qm7w-skx0-OsTz9z
   
  --- Physical volume ---
  PV Name               /dev/md1
  VG Name               v_g
  PV Size               1018.00 MiB / not usable 2.00 MiB
  Allocatable           yes 
  PE Size               4.00 MiB
  Total PE              254
  Free PE               254
  Allocated PE          0
  PV UUID               QaafMa-OqOG-M3xx-8AYF-xGFr-rFcv-B0hKeW
   
  --- Physical volume ---
  PV Name               /dev/md0
  VG Name               v_g
  PV Size               <2.00 GiB / not usable 0   
  Allocatable           yes 
  PE Size               4.00 MiB
  Total PE              511
  Free PE               511
  Allocated PE          0
  PV UUID               8ObfJ3-p3Ee-IaKC-rHn3-FfyS-CEWb-jSiIeT

10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

vagrant@vagrant:~$ sudo lvcreate -L 100M -n LV100 v_g /dev/md1
  Logical volume "LV100" created.
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk  
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part  
└─sda5                 8:5    0 63.5G  0 part  
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk  
├─sdb1                 8:17   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1 
└─sdb2                 8:18   0  511M  0 part  
  └─md1                9:1    0 1018M  0 raid0 
    └─v_g-LV100      253:2    0  100M  0 lvm   
sdc                    8:32   0  2.5G  0 disk  
├─sdc1                 8:33   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1 
└─sdc2                 8:34   0  511M  0 part  
  └─md1                9:1    0 1018M  0 raid0 
    └─v_g-LV100      253:2    0  100M  0 lvm   

11. Создайте mkfs.ext4 ФС на получившемся LV.

vagrant@vagrant:~$ sudo mkfs.ext4 /dev/v_g/LV100
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 25600 4k blocks and 25600 inodes

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done

vagrant@vagrant:~$ lsblk -f
NAME        FSTYPE LABEL  UUID                                   FSAVAIL FSUSE% MOUNTPOINT
sda                                                                             
├─sda1      vfat          7D3B-6BE4                                 511M     0% /boot/efi
├─sda2                                                                          
└─sda5      LVM2_m        Mx3LcA-uMnN-h9yB-gC2w-qm7w-skx0-OsTz9z                
  ├─vgvagrant-root
  │         ext4          b527b79c-7f45-4e2b-a90f-1a4e9cb477c2     56.7G     2% /
  └─vgvagrant-swap_1
            swap          fad91b1f-6eed-4e4b-8dbf-913ba5bcacc7                  [SWAP]
sdb         linux_ vagrant:0
│                         d37c6081-c373-5ddf-3864-c3ccd347c462                  
├─sdb1      linux_ vagrant:0
│                         972a96e1-ce7b-bde4-5fa0-a9b1a83196d1                  
│ └─md0     LVM2_m        8ObfJ3-p3Ee-IaKC-rHn3-FfyS-CEWb-jSiIeT                
└─sdb2      linux_ vagrant:1
                          221a13bb-6ea9-19d2-3d1e-2c6b5a30b4f2                  
  └─md1     LVM2_m        QaafMa-OqOG-M3xx-8AYF-xGFr-rFcv-B0hKeW                
    └─v_g-LV100
            ext4          b98f4fa6-1034-49d8-aca1-a167fdb54a55                  
sdc         linux_ vagrant:0
│                         d37c6081-c373-5ddf-3864-c3ccd347c462                  
├─sdc1      linux_ vagrant:0
│                         972a96e1-ce7b-bde4-5fa0-a9b1a83196d1                  
│ └─md0     LVM2_m        8ObfJ3-p3Ee-IaKC-rHn3-FfyS-CEWb-jSiIeT                
└─sdc2      linux_ vagrant:1
                          221a13bb-6ea9-19d2-3d1e-2c6b5a30b4f2                  
  └─md1     LVM2_m        QaafMa-OqOG-M3xx-8AYF-xGFr-rFcv-B0hKeW                
    └─v_g-LV100
            ext4          b98f4fa6-1034-49d8-aca1-a167fdb54a55   
            
12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.

vagrant@vagrant:~$ mkdir /tmp/new
vagrant@vagrant:~$ sudo mount /dev/v_g/LV100 /tmp/new
vagrant@vagrant:~$ findmnt --real
TARGET      SOURCE                     FSTYPE  OPTIONS
/           /dev/mapper/vgvagrant-root ext4    rw,relatime,errors=remount-ro
├─/sys/kernel/tracing
│           tracefs                    tracefs rw,nosuid,nodev,noexec,relatime
├─/boot/efi /dev/sda1                  vfat    rw,relatime,fmask=0077,dmask=0077,codepage=
├─/vagrant  vagrant                    vboxsf  rw,nodev,relatime,iocharset=utf8,uid=1000,g
└─/tmp/new  /dev/mapper/v_g-LV100      ext4    rw,relatime,stripe=256

13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.

sudo wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz
--2022-04-10 19:21:27--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183
Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 22262449 (21M) [application/octet-stream]
Saving to: ‘/tmp/new/test.gz’

/tmp/new/test.gz       100%[==========================>]  21.23M  3.40MB/s    in 7.5s    

2022-04-10 19:21:34 (2.82 MB/s) - ‘/tmp/new/test.gz’ saved [22262449/22262449]

vagrant@vagrant:~$ ls -lha /tmp/new/
total 22M
drwxr-xr-x  3 root root 4.0K Apr 10 19:21 .
drwxrwxrwt 11 root root 4.0K Apr 10 19:18 ..
drwx------  2 root root  16K Apr 10 19:02 lost+found
-rw-r--r--  1 root root  22M Apr 10 18:57 test.gz

14. Прикрепите вывод lsblk.

vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk  
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part  
└─sda5                 8:5    0 63.5G  0 part  
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk  
├─sdb1                 8:17   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1 
└─sdb2                 8:18   0  511M  0 part  
  └─md1                9:1    0 1018M  0 raid0 
    └─v_g-LV100      253:2    0  100M  0 lvm   /tmp/new
sdc                    8:32   0  2.5G  0 disk  
├─sdc1                 8:33   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1 
└─sdc2                 8:34   0  511M  0 part  
  └─md1                9:1    0 1018M  0 raid0 
    └─v_g-LV100      253:2    0  100M  0 lvm   /tmp/new

15. Протестируйте целостность файла:

vagrant@vagrant:~$ gzip -t /tmp/new/test.gz
vagrant@vagrant:~$ echo $?
0

vagrant@vagrant:~$ gzip -t /tmp/new/test.gz && echo $?
0

16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.

vagrant@vagrant:~$ sudo pvmove /dev/md1
  /dev/md1: Moved: 16.00%
  /dev/md1: Moved: 100.00%
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk  
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part  
└─sda5                 8:5    0 63.5G  0 part  
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk  
├─sdb1                 8:17   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1 
│   └─v_g-LV100      253:2    0  100M  0 lvm   /tmp/new
└─sdb2                 8:18   0  511M  0 part  
  └─md1                9:1    0 1018M  0 raid0 
sdc                    8:32   0  2.5G  0 disk  
├─sdc1                 8:33   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1 
│   └─v_g-LV100      253:2    0  100M  0 lvm   /tmp/new
└─sdc2                 8:34   0  511M  0 part  
  └─md1                9:1    0 1018M  0 raid0 

17. Сделайте --fail на устройство в вашем RAID1 md.

vagrant@vagrant:~$ sudo mdadm /dev/md0 --fail /dev/sdb1  
mdadm: set /dev/sdb1 faulty in /dev/md0

18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.

vagrant@vagrant:~$ dmesg | grep md0
[ 4885.169180] md/raid1:md0: not clean -- starting background reconstruction
[ 4885.169181] md/raid1:md0: active with 2 out of 2 mirrors
[ 4885.169199] md0: detected capacity change from 0 to 2681208832
[ 4885.171816] md: resync of RAID array md0
[ 4898.390871] md: md0: resync done.
[ 5203.603854] md0: detected capacity change from 2681208832 to 0
[ 5203.603860] md: md0 stopped.
[ 5247.842359] md/raid1:md0: not clean -- starting background reconstruction
[ 5247.842361] md/raid1:md0: active with 2 out of 2 mirrors
[ 5247.842383] md0: detected capacity change from 0 to 2144337920
[ 5247.844080] md: resync of RAID array md0
[ 5258.356683] md: md0: resync done.
[10488.810729] md/raid1:md0: Disk failure on sdb1, disabling device.
               md/raid1:md0: Operation continuing on 1 devices.
               
19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:   

vagrant@vagrant:~$ gzip -t /tmp/new/test.gz && echo $?
0

20. Погасите тестовый хост, vagrant destroy.    


