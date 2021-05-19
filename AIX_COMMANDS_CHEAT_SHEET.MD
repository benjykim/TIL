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
| Hardware Info                          | `lscfg -v` / `lscfg -l ent0` / `lscfg -vl fcs0 (find the WWN of HBA adapter)` / `lsdev ` / `lsdev -Cc disk -p scsi0` / `lsslot -c [pci\|phb\|port]` / `lspath -l hdisk0` |
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


### 네트워크
| 구분 | 명령어 |
|---|---|
| Displaying network interfaces | `ifconfig -a` / `entstat -d <interface>` / `lsdev -Cc if` / `odmget -q "name=en0" CuAt` / `lsattr -EHl en0` |
| Configure network interface | `mktcpip (completely setup a network interface)` / `rmtcpip (remove all network interfaces)` / `mktcpip -h aix1 -a 192.168.1.200 -m 255.255.255.0 -i en1 -g 192.168.0.10` |
|  | `-h = hostname assigned to interface` / `-a = ip address` / `-m = netmask` / `-i = interface name` / `-g = gateway ip address` |
|  | `ifconfig en1 detach ← remove an interface` / `chdev (add aliases to network interface` |
| Starting and stopping a network interface | `ifconfig en0 up` / `ifconfig en0 down` / `ifconfig en0 detach (remove)` |
| Setting NIC speed | `chdev -l ent0 -a media_speed=1000_Full_Duplex -P` / `entX = physical device` / `enX = frame type run on entX` |
| Change NIC parameters | `netstat -v` / `entstat -d <interface>` / `no -a` / `no -o "ipforwarding=1"` |
| Display NIC statistics | `netstat -i [-f inet]` / `netstat -s ` / `entstat -d <interface>` |
| Display MAC address | `netstat -ia` |
| Displaying network packets | `tcpdump -i <interface>` / `iptrace -i <interface> <output file>` / `ipreport (used with iptrace to view reports)` / `Note : you must stop the iptrace by using "kill -15"` |
| Default router | `route add 0 <gateway IP address>` / `Note : there is no file that holds the default router` |
| Display routing table | `netstat -rn` / `netstat -r -f inet` / `lsattr -EHl inet0 -a route` |
| Test IPMP, Bonding | `smitty etherchannel (creates, deletes and tests)` / `entstat -d ent0` |
| Change the hostname | `hostname <new hostname>` / `chdev -l inet0 -a hostname=<hostname>` |
| Setup DNS | `/etc/resolv.conf` |
| Name service switch file (DNS client) | `/etc/netsvc.conf` / `chnamsv (change name service)` / `rmnamsv (remove a name service)` / `lsnamsv -C (list name services)` |
| Flush DNS cache | `netcdctrl -t dns -e hosts -f` |
| Domain Name | `domainname <domainname>` |
| Obtain IP Address routing | `route -n get <hostname>` |
| Find Services on the network | `rpcinfo -b bootparam 1 (Boot(jumpstart) servers)` / `rpcinfo -b mountd 1 (NFS servers)` / `rpcinfo -b ypserv 1 (NIS servers/slaves)`  |

### 크래시 덤프
| 구분 | 명령어 |
|---|---|
| Crash Dump | `sysdumpdev -l (list dump destination)` / `sysdumpdev -e (estimates dumpsize)`  / `sysdumpdev -L (info)` |

### 성능 모니터링 및 진단
| 구분 | 명령어 |
|---|---|
| CPU | `topas -P` / `topas -L (logical partitions)` / `mpstat` / `sar -c` / `w` / `lparstat` / `tprof` / `curt` |
| Memory | `topas` / `vmstat` / `sar -b` / `svmon` / `rmss`/ `ipcs -a` / `ps` |
| Network | `[ent\|tok\|fddi\|atm]stat` / `netstat` / `netpmon (trcstop to stop trace)` |
| Disk | `topas -D (disk)` / `topas -F (filesystem)` / `iostat` / `sar -D` / `fcstat (fibre)` / `lvmstat` / `filemon (trcstop to stop)` / `fileplace` / `chdev -l sys0 -a iostat=true ← disk stat history` |
| Application | `topas` / `truss` / `sar` / `probevue` / `tprof` |
| NFS | `nfsstat` |

### 커널 모듈 및 파라미터
| 구분 | 명령어 |
|---|---|
| Display loaded modules | `genkex` |
| Set kernel parameters (tuning) | `chdev -l sys0 -a <parameter>=<value>` / `no -a (network)` / `vmo -a (virtual memory)` / `nfso -a (NFS)` / `ioo -a (Input/Output)` / `raso -a (reliability, availability, serviceability)` |
|  | `schedo -a (processor scheduler)` / `vi /etc/security/limits` / `cd /etc/tunables` |
| Display kernel parameters | `lsattr -EHl sys0` |
| Build kernel | `chdev -l sys0 -a <parameter>=<value>`|
|  | `Note : most params are dynamically changed in AIX, for example memory segments are dynamically adjusted` |
| Interprocess communication | `ipcs -a` |

### 서비스
| 구분 | 명령어 |
|---|---|
| Display services | `lssrc -a` |
| Start services | `startsrc -s <subsystem>` / `startsrc -g <group>` |
| Stop services | `stopsrc -s <subsystem>` / `stopsrc -g <group>` |
| Reload service | `refresh -s <subsystem>` |
| Restart service | `stopsrc -s <subsystem>` / `startsrc -s <subsystem>` |
| Service status | `lssrc -a` |
| Service logging, etc | `/var/adm/ras` / `/etc/syslog.conf` / `etc/rc.tcpip` |


### 패치 및 소프트웨어
| 구분 | 명령어 |
|---|---|
| Display installed patches | `instfix -ia` |
| Adding patch | `instfix -k` |
| Removing patch | `installp -r` |
| Display installed packages | `lslpp -L all (all filesets)` / `lslpp -L <package> (single fileset)` / `lslpp -w <file> (file belongs)` / `lslpp -ha (history of filesets)`|
|  | `rpm -qa (all packages)` / `rpm -q (single package)` / `rpm -qf (file belongs)` / `rpm -qi <package> (very detailed)` |
|  | `oslevel -g (install packages above os level)` / `whereis <filename>` / `which_fileset <filename>` |
| adding package | `installp -a` / `installp -c (cleanup after failed install)` / `rpm -i` |
| removing packages | `installp -u (committed packages)` / `installp -r (applied packages)` / `rpm -e <package>` |
| verify package | `lppchk -v` / `rpm -V <package>` |
| List files in package | `lslpp -f <fileset>` / `rpm -ql <package>` |
| Package directory | `/usr/lpp` / `/var/lib/rpm` |
| List libraries required for binary program  | `ldd <file>` |

### 계정
| 구분 | 명령어 |
|---|---|
| Display users | `cat /etc/passwd` / `lsuser -f ALL (detailed)` |
| Create a user | `mkuser` / `useradd` |
| Remove a user | `rmuser` / `userdel` |
| Modify a user | `chuser -a` / `usermod` / `passwd -f` / `chfn <username>` |
| Change user password | `passwd` / `pwdadm` / `pwdck -t ALL` |
| Create a group | `mkgroup <group name>` |
| Remove a group | `rmgroup <group name>` |
| Modify a group | `chgroup <attribute> <groupname>` |
| Password files | `/etc/security/passwd` |
| Useful user commands | `id` / `whoami` / `who` / `finger` |
| Useful group commands | `groups` / `setgroups` / `lsgroup ALL` |

### NFS
| 구분 | 명령어 |
|---|---|
| NTP Daemons | `server : rpc.mountd, nfsd` / `client : rpc.statd, rpc.lockd` |
| NFS files | `/etc/exports` / `/etc/xtab` |
| List NFS clients that have a remote mount | `/etc/xtab` |
| Display NFS shares | `exportfs` / `showmount -e localhost` |
| Create NFS share | `mknfsexp -d <directory>` / `mknfsmnt` / `shareall` |
| Uncreate NFS share | `rmnfsexp -d <directory> (unshares and removes from file)` / `exportfs -u <filesystem>` / `unshareall` |
| Start/change NFS daemons | `mknfs` / `chnfs` / `startsrc -s nfsd` / `startsrc -s rpc.mountd` |
| Stop NFS daemons | `rmnfs` / `stopsrc -s nfsd` / `stopsrc -s rpc.mountd` |
| NFS status | `lssrc -a \| grep -i nfs` |
| NFS reload | `exportfs -av` |
| NFS performance | `nfsstat` |
| NFS Options | `nfso -a` / `nfso -o <option>=<value>` |

### NTP
| 구분 | 명령어 |
|---|---|
| Time daemons | `xntpd` |
| NTP setup | `/etc/ntp.conf` / `startsrc -s xntpd` / `stopsrc -s xntpd` / `lslpp -L all \| grep xntpd`|
| NTP daemon options | `startsrc -s xntpd -a "-x"` / `/etc/rc.tcpip` |
| NTP Trace commands | `ntpq -p` / `ntptrace` / `ntpdate` |

### 로그 파일
| 구분 | 명령어 |
|---|---|
| messages | `/var/adm/ras` |
| syslog | `/var/adm/ras` |
| mail | `/usr/spool/mqueue` |
| cron | `/var/adm/cron/log` |
| boot | `/var/adm/ras` / `alog -o -t boot` / `alog -o -t console` |
|  | `alog -L (list all the logs available)` |
| Error logging | `/usr/lib/errdemon -l (display attributes)` / `/usr/lib/errdemon (start error logging)` / `/usr/lib/errstop (stop error logging)` |
|  | `errpt (summary errorlog report)` / `errpt -a (detailed errorlog report)` / `errclear (clears errorlog)` |

### 보안
| 구분 | 명령어 |
|---|---|
| Checking the passwd file | `pwdck -t ALL` / `usrck -t ALL` |
| Checking the group file | `grpck` |
| Console login (allow/deny) | `/etc/security/user` / `chsec -f /etc/security/user -s root` |
