# AIX 명령어 정리

### 목차
* [시스템 정보](#시스템-정보)
* [메모리 및 스왑](#메모리-및-스왑)
* [디스크, 파일-시스템 및 디바이스](#디스크-파일-시스템-및-디바이스)
* [네트워크](#네트워크)
* [크래시 덤프](#크래시-덤프)
* [성능 모니터링 및 진단](#성능-모니터링-및-진단)
* [커널 모듈 및 파라미터](#커널-모듈-및-파라미터)
* [패치 및 소프트웨어](#패치-및-소프트웨어)
* [계정](#계정)
* [NFS](#nfs)
* [NTP](#ntp)
* [로그 파일](#로그-파일)
* [보안](#보안)

---

### 시스템 정보
| 구분                                   | 명령어                                                       |
| -------------------------------------- | ------------------------------------------------------------ |
| Server Release Info                    | `oslevel -r`                                                 |
| Server type                            | `prtconf \| grep -i 'System Model'`                          |
| Hardware Info                          | `lscfg -v` / `lscfg -l ent0` / `lscfg -vl fcs0 (find the WWN of HBA adapter)` / `lsdev ` / `lsdev -Cc disk -p scsi0` / `lsslot -c [pci|phb|port]` / `lspath -l hdisk0` |
| Operating System                       | `oslevel [-r\|-s]`                                           |
| Memory                                 | `prtconf -m` / `prtconf \| grep -i memory` / `lsattr -El sys0 -a realmem` / `bootinfo -r` |
| CPU(type, number, etc)                 | `prtconf \| grep -i processor`                               |
| Disk Drives                            | `lsdev -Cc disk` / `lsdev -Cc disk -p scsi0 (specific controller)` / `lsdev -Cc disk -S [a\|d\|s] (available, defined, stopped)` / `lscfg -v -l hdisk0` |
| Kernel File and assoicated directories | `/unix`  / `/usr/lib/boot` / `/usr/lib/drivers` / `/usr/lib/boot/unix_64` |
| Kernel 32 or 64                        | `prtconf -k` / `bootinfo -K`                                 |
| Display Firmware                       | `prtconf \| grep -i firmware` / `lscfg -pv` / `invscout`     |
| Display IRQ, IO ports and DMA          | `prtconf`                                                    |
| GUI admin tool                         | `smit` / `smitty`                                            |


### 메모리 및 스왑
| 구분              | 명령어                                                       |
| ----------------- | ------------------------------------------------------------ |
| Memory            | `prtconf -m` / `prtconf \| grep -i memory` / `lsattr -El sys0 -a realmem` / `bootinfo -r` |
| Page size(memory) | `pagesize` / `pagesize -a (display all supported pagesizes)` |
| Display swap      | `lsps -a (detailed)` / `lsps -s`                             |
| Adding swap       | `mkps -a -s 4 -n <volume group>` / `chps -a n paging00 (don't use after restart) ← change the attributes` / `chlv -n NEW_NAME OLD_NAME (change page space name) ← change the logical volume attributes (name in this case)` |
|                   | `-a = reconfigure paging space after restart` / `-s = size of the page space (logical partitions)` / `-n = activates the paging space (use swapoff to deactivate)` |
| Removing swap     | `swapoff /dev/paging00` / `rmps paging00` / `Note: paging space must be activated before removing` |

### 디스크, 파일 시스템 및 디바이스
| 구분                          | 명령어                                                       |
| ----------------------------- | ------------------------------------------------------------ |
| Disk Drives                   | `lsdev -Cc disk`                                             |
| Disk Serial Number, type, etc | `lscfg -vl hdisk0`                                           |
| Disk partitions               | `lsvg -l rootvg` / `lchangelv` / `cat /etc/filesystems`      |
| List Raw Partitions           | `mklv -y rawVolume vg01 10 ← create a raw volume`            |
| Bad Blocks                    | `chlv -b [y\|n] <lv> ← Note : enables bad block relocation`  |
| Filesystem commands           | `df -k` / `lsfs [<filesystem>]` / `lsfs -q <filesystem> (detailed)` |
| Filesystem (create)           | `crfs -v jfs2 -d data02lv -m /data02 -A yes` / `-v = filesystem type, -d = device of logical volume, -m = mountpoint, -A = mount after restart [yes\|no]` |
| Filesystem (remove)           | `rmfs -ri /data02` / `-r = remove the mountpoint, -i = display warning before removing` |
| Tune Filesystems              | `chfs` / `Note : you can perform the following - resize, freeze, change mountpoint, permissions...` |
| Backup filesystem             | `backup \|  restore` / `tar` / `dd` / `cpio`                 |
| Display the boot device       | `bootinfo -b (display last boot device)` / `bootlist -m [normal\|sevice] -o (display bootable devices)` |
| Setting the boot device       | `bootlist -m normal hdisk0 hdisk1`                           |
| Createing boot device (MBR)   | `bosboot -a (uses default device)` / `bosboot -ad hdisk1`    |
| mount/unmount floppy          | `mount /dev/rfd0 /floppy`                                    |
| mount/unmount CDROM           | `mount -v cdrfs -r /dev/cd0 /cdrom` / `unmount /cdrom`       |
| Update /dev directory         | `cfgmgr` / `cfgmgr -l scsi0` / `mkdev`                       |
| Remove or change a device     | `rmdev -l cd0` / `chdev -l rmt0 -a ret=no`                   |

|      |      |
|---|---|
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
