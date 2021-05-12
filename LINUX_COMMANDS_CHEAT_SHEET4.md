### 하드웨어

### 하드웨어
| 명령어 | 설명 | 예시 | 
|---|---|---|
| dmesg | print or control the kernel ring buffer |  |
| lsusb | List USB devices |  |
| lspci | list all PCI devices |  |
| nproc | print the number of processing units available |  |
| inxi | Display info about all hardware |  |
| hwinfo | Display info about all hardware |  |
| lshw | Display info about all hardware  |  |
| lscpu | Display all CPU info |  |

#### 모듈
| 명령어 | 설명 | 예시 | 
|---|---|---|
| lsmod | show the status of modules in the Linux Kernel |  |
| modinfo | show information about a Linux Kernel module |  |
| insmod | insert a module into the Linux Kernel |  |
| rmmod | remove a module from the Linux Kernel |  |
| modprobe | add and remove modules from the Linux Kernel |  |

### 디스크 유틸(Utilities)

####  HDD
| 명령어 | 설명 | 예시 | 
|---|---|---|
| du | estimate file space usage |  |
| fuser | identify processes using files or sockets |  |
| chroot | run command or interactive shell with special root directory |  |
| hdparm | get/set hard disk parameters |  |
| dumpe2fs | dump ext2/ext3/ext4 filesystem information |  |
| badblocks | search a device for bad blocks |  | 
#### 파티션
| 명령어 | 설명 | 예시 | 
|---|---|---|
| df | report file system disk space usage |  |
| sfdisk | partition table manipulator for Linux |  |
| fdisk | manipulate disk partition table |  |
| gdisk | Interactive GUID partition table (GPT) manipulator |  |
| parted | a partition manipulation program |  |
| lsblk | list block devices |  |
| e2label | Change the label on an ext2/ext3/ext4 filesystem |  |

#### Swap
| 명령어 | 설명 | 예시 | 
|---|---|---|
| mkswap | set up a Linux swap area |  |
| swapon | enable devices and files for paging and swapping |  |
| swapoff | disable devices and files for paging and swapping |  |

#### 파일 시스템
| 명령어 | 설명 | 예시 | 
|---|---|---|
| mkfs | build a Linux filesystem <br/> #you must umount the device before mkfs) |  |
| mkfs.xfs <br/> mkfs.ext4 <br/> mkfs.ext3 | #you must umount the device before mkfs. |  |
| mkfs2fs | create an ext2/ext3/ext4 filesystem <br/> #you must umount the device before mkfs |  |
| tune2fs | adjust tunable filesystem parameters on ext2/ext3/ext4 filesystems |  |
| fsck | check and repair a Linux filesystem you must umount the device before fsck. for example single usermode and umount. ('shutdown -r -F now' is force fsck after reboot.) |  |
| fsck.ext4 | check and repair a Linux filesystem |  |
| e2fsck | check a Linux ext2/ext3/ext4 file system |  |
| resize2fs |ext2/ext3/ext4 file system resizer |  |

#### 데이터
| 명령어 | 설명 | 예시 | 
|---|---|---|
| dd | convert and copy a file |  |
| sync | flush file system buffers |  |
| shred | overwrite a file to hide its contents, and optionally delete it |  |

#### 마운트
| 명령어 | 설명 | 예시 | 
|---|---|---|
| mount | mount a filesystem |  |
| umount | unmount file systems |  |

### 성능

#### 성능
| 명령어 | 설명 | 예시 | 
|---|---|---|
| top | display Linux processes |  |
| sar | Collect, report, or save system activity information |  |
| vmstat | Report virtual memory statistics |  |
| iostat | Report Central Processing Unit (CPU) statistics and input/output statistics for devices and partitions. |  |
| mpstat | Report processors related statistics. |  |
| uptime | Tell how long the system has been running |  |
| w | Show who is logged on and what they are doing |  |
| free | Display amount of free and used memory in the system |  |
| netstat | Print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships |  |
| iotop | simple top-like I/O monitor |  |
| dstat | versatile tool for generating system resource statistics |  |

---

* 출처 : https://cmdref.net/os/linux/command/index.html#schedule
