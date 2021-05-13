# LINUX (RHEL/CENTOS) 명령어 정리
### 목차

* 시스템 명령어
	* [시스템 명령어](#시스템-명령어)
	* [프로세스 관리](#프로세스-관리)
	* [스케줄](#스케줄)
	* [시스템 ETC](#시스템-etc)
* 파일 및 디렉토리 명령어
	* [파일 명령어](#파일-명령어)
	* [디렉터리 명령어](#디렉터리-명령어)
	* [파일 내용 접근 명령어](#파일-내용-접근-명령어)
	* [검색](#검색)
* 유저 관리(Administration)
	* [유저](#유저)
	* [그룹](#그룹)
	* [파일 권한](#파일-권한)
	* [유저 ETC](#유저-etc)
* 네트워크
	* [네트워크](#네트워크)
	* [네트워크 연결 확인](#네트워크-연결-확인)
	* [DNS](#dns)
	* [연결](#연결)
	* [HTTP](#http)
* 하드웨어
	* [하드웨어](#하드웨어)
	* [모듈](#모듈)
* 디스크 유틸(Utilities)
	* [HDD](#hdd)
	* [파티션](#파티션)
	* [Swap](#swap)
	* [파일 시스템](#파일-시스템)
	* [데이터](#데이터)
	* [마운트](#마운트)
* 성능
	* [성능](#성능)

---

### 시스템 명령어
#### 시스템 명령어
| 명령어 | 설명 | 예시 |
|---|---|---|
| shutdown | bring the system down | shutdown -h now ← -h = Halt or poweroff after shutdown <br/> shutdown -r now ← -r = Reboot after shutdown <br/> shutdown -r -F now ← -F = Force fsck after reboot.  |
| halt | stop the system | halt |
| reboot | reboot the system. | reboot |
| init |  | init 1 #change to single usermode |
| uptime | Tell how long the system has been running. | uptime |
| runlevel | find the previous and current system runlevel. | runlevel |
| printenv | print all or part of environment | printenv |
| env | run a program in a modified environment | env |
| hostname | show or set the system's host name | hostname ← show the system's host name <br/> I recommend  `uname -n`  for check hostname. <br/> hostname NEWHOSTNAME ← set the system's host name |
| uname | print system information | uname -a ← print all information (=uname --all) <br/> uname -n ← show the system's host name (=uname --nodename) |
| locale | Get locale-specific information. | locale <br/> locale -a |grep -i ja <- -a : --all-locales |

#### 프로세스 관리
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ps | report a snapshot of the current processes | ps aux \| grep httpd ← Check httpd process <br/> ps aux \| grep XXX \| awk '{print $2}' \| xargs kill -9 <br/> ps auxwf <br/> ps auxwf \| grep XXX |
| pgrep | look up processes based on name and other attributes | pgrep -f 'bash' <br/> pgrep -lf 'bash' ← output with process name <br/> pgrep -f 'bash \| xargs kill |
| pstree | display a tree of processes | pstree -a |
| pidof | find the process ID of a running program | pidof httpd <br/> /bin/kill $(/sbin/pidof qmail-popup) |
| kill | send a signal to a process | kill -9 PID ← -9 or -KILL = force-quit |
| pkill | signal processes based on name and other attributes | pkill -f 'bash' <br/> pkill -u user1 <br/> pkill java <br/> pkill -f jar |
| killall | kill processes by name | killall vi <br/> killall -i vi ← -i = interactively <br/> killall -HUP kterm |
| killproc |  |  |
| lsof | list open files | lsof -i <br/> lsof -i -P ← no port names <br/> lsof -i :80,443 ← Which process is using Port 80,443 |
| Ctrl + C | Stop running process |  |
| Ctrl + Z | Suspend running process | Move Running Process to Background <br/> 1. ctrl + z <br/> 2. jobs <br/> 3. bg <br/> 4. disown %JOBID |
| jobs | The first form lists the active jobs. |  |
| fg |Resume jobspec in the foreground  |  |
| bg | Resume each suspended job jobspec in the background | jobs -l ← List job |
| nohup | run a command immune to hangups, with output to a non-tty | nohup command.sh & |
| disown |  | disown %JOBID |
| nice | run a program with modified scheduling priority | nice -n 19 test.sh <br/> nice -n 19 ionice -c 3 CMD <br/> nice -n 19 ionice -c 2 -n 7 COMMAND |
| renice | alter priority of running processes | renice 19 -p PID <br/> you can check the nice with "top" or "ps alx" |
| ionice | sets or gets process io scheduling class and priority | ionice -p PID ← check <br/> ionice -c 3 -p PID <br/> nice -n 19 ionice -c 2 -n 7 COMMAND |

#### 스케줄
| 명령어 | 설명 | 예시 |
|---|---|---|
| crontab | maintain crontab files | crontab -l ← -l = list user's crontab <br/> crontab -u USER -l <br/> crontab -e ← -e = edit user's crontab <br/> crontab -u USER -e  |
| at | queue jobs for later execution | echo "/sbin/shutdown -h now" \| at 21:00 02/30/2009 <br/> at -t 200902302100  |
| atq | lists the user's pending jobs | atq |
| atrm | delete jobs for later execution | atrm JOBID |
| watch | execute a program periodically, showing output fullscreen | watch ntpq -p ← By default, the program is run every 2 seconds <br/> watch -n 1 ntpq -p ← 1 seconds interval <br/> -d = highlight the differences between successive updates|


#### 시스템 ETC
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ntpupdate | set the date and time via NTP | ntpupdate -b -u IP <br/> -b = Force the time(step mode) <br/> -u = If you are running ntpd, "-u" must be added |
| history | GNU History Library | history \| less <br/> history 5 ← lists only the last 5 lines <br/> HISTSIZE=1000 <br/> HISTTIMEFORMAT="%Y/%m/%d  %H:%M:%S" |
| which | locate a command | which ls |
| strace | trace system calls and signals | strace -t php test.php <br/> strace -t -o test.txt php test.php <br/> -t = each line of the trace with the time of day |
| ltrace | A library call tracer | ltrace -o test.txt wget http://example.com/ <br/> ltrace -p PID <br/> ltrace -p 3365 |
| script | make typescript of terminal session | script -afq $LOG |

### 파일 및 디렉토리 명령어
#### 파일 명령어
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ls | list directory contents | ls -ltrh ← -r = reverse order while sorting <br/> -h = with -l, print sizes in human readable format <br/> ls -ltrh \| less |
| cp | copy files and directories | cp -p SRC DES <br/> cp -pr SRC/ DES/ ← -r, -R = copy directories recursively <br/> cp -f SRC DES ← -f, --force|
| mv | move (rename) files | mv file1 file2 <br/> mv dir1 dir2 <br/> mv file1 file2 DIR |
| rename | rename multiple files | rename 's/(.변경하고자 하는 text)/(.변경할 text)' (찾을 파일) <br/> rename 's/.txt/.c/' *.txt ← *.txt 파일을 모두 *.c파일로 변경하기 <br/> rename -n 's/test/ttt/' *.txt ← -n = 변경될 파일 미리 출력 가능(검증용)  <br/> rename -v 's/test/ttt/' *.txt ← -v = 변경 및 변경된 파일 출력 |
| rm | remove files or directories  | rm -rf TARGET ← -r, -R = remove directories |
| touch | change file timestamps | touch file1 <br/> touch -d "2021/10/20 13:00:00" file1 |
| ln | make links between files | ln -s SRC DES |
| unlink |  | unlink DES |
| wc | print newline, word, and byte counts for each file | wc -l ← -l, --lines = print the line counts |
| tree | list contents of directories in a tree-like format | tree -Dpuga /etc |
| col | filter reverse line feeds from input | man ifconfig \| col -bfx > test.txt |

#### 디렉터리 명령어
| 명령어 | 설명 | 예시 | 
|---|---|---|
| pwd | print name of current/working directory | pwd |
| cd | Change the current directory | cd ~ <br/> cd .. |
| pushd | Adds a directory to the top of the directory stack | pushd /var/log <br/> pushd \`pwd\` |
| popd | Removes entries from the directory stack | popd |
| dirs | displays the list of currently remembered directories | dirs -v |
| mkdir | make directories | mkdir -p /tmp/test1/test2/ ← make parent directories as needed <br/> mkdir -m 700 /home/user01/.ssh |
| rmdir | remove empty directories <br/> If you want to delete directory, you must use “rm -r DIR” | rmdir DIR |

#### 파일 내용 접근 명령어
| 명령어 | 설명 | 예시 | 
|---|---|---|
| more | file perusal for crt viewing |  |
| less | opposite of more | crontab -l \| less |
| view | Start in read-only mode |  |
| cat | concatenate files and print on the standard output | cat /dev/null > access.log |
| tail | output the last part of files | tail -n 50 a.txt ← output the last N lines <br/> tail -f /var/log/messages |
| tailf | follow the growth of a log file | head -n 100 a.txt ← -n, --lines |
| head | output the first part of files |  |
| diff | compare files line by line | diff --suppress-common-lines --side-by-side File1 File2 <br/> diff -r dir1 dir2 ← When comparing directories, recursively compare |
| sdiff | side-by-side merge of file differences | sdiff -s File1 File2 ← -s = Do not print common lines <br/> sdiff -s -w 200 File1 File2 |
| colordiff |  |  |
| vimdiff |  | vimdiff File1 File2 <br/> vim -d File1 File2 |

#### 검색
| 명령어 | 설명 | 예시 | 
|---|---|---|
| grep | print lines matching a pattern | grep WORD FILE \| less <br/> grep -Ev "^#\|^$" xxx.tx <br/> grep . ifcfg-eth* ← check filename and contents <br/> grep -r PATTERN --include="*.txt" DIRECTORY ← -r = recursive  |
| egrep | egrep is the same as grep -E | egrep "aaa\|bbb" file |
| find, xargs | search for files in a directory hierarchy | find . -name "*txt*"  |

### 유저 관리(Administration)

#### 유저
| 명령어 | 설명 | 예시 | 
|---|---|---|
| useradd | create a new user or update default new user information | useradd -G wheel USER1 |
| adduser | add a user to the system |  |
| whoami | print effective userid | whoami |
| w | Show who is logged on and what they are doing | w |
| who | show who is logged on | who |
| userdel | delete a user account and related files | userdel -r USER ← -r, --remove = Files in the user's home directory will be removed |
| vipw | edit the password, group, shadow-password or shadow-group file | vipw ← edit /etc/passwd <br/> vipw -s ← edit /etc/shadow |
| passwd | change user password | passwd -S USER01 ← check about the status of the password <br/> passwd -l USER01 ← locc the user <br/> passwd -u USER01 ← unlock  |
| chpasswd | update passwords in batch mode | echo USER01:password \| chpasswd |
| chage | change user password expiry information | chage -l USER ← check <br/> chage -M 90 USER ← the password expires day set 90 days |
| usermod | modify a user account | usermod -g GROUP USER <br> usermod -l NEW_USERNAME OLD_USERNAME ← change username <br/> usermod -u UID USER ← change UID|
| chsh | change login shell | chsh -s /bin/bash ← changing shell |
| getent | get entries from Name Service Switch libraries | getent passwd ← you can check LDAP users |
| pam_tally2 | The login counter (tallying) module | pam_tally2 -u USER ← check <br/> pam_tally2 -u USER --reset ← reset |

#### 그룹
| 명령어 | 설명 | 예시 | 
|---|---|---|
| groups | print the groups a user is in | groups <br/> groups USERNAME |
| groupadd | create a new group | groupadd -g GIP GROUP <br/> groupadd -g 1100 dev |
| addgroup | add group to the system | addgroup [--gid ID] group |
| groupdel | delete a group | groupdel GROUP |
| groupmod | change USER's GID | groupmod -g GID GROUP <br/> groupmod -g 1501 testgroup1 |
| chgrp | change the Group of the file | chgrp -R GROUP FILE |
| vigr | edit the password, group, shadow-password or shadow-group file |  |

#### 파일 권한
| 명령어 | 설명 | 예시 | 
|---|---|---|
| chmod | change file mode bits | chmod 777 TARGET <br/> chmod u+s PROGRAM ← add SSUID(Set User ID) |
| chown | change file owner and group | chown USER FILE <br/> chown USER:GROUP FILE |

#### 유저 ETC
| 명령어 | 설명 | 예시 | 
|---|---|---|
| finger | user information lookup program | finger USER01 |
| su | change user ID or become superuser | su - ← change root user <br/> sudo su - USER -s /bin/bash |
| sudo | execute a command as another user | sudo -u USER COMMAND |
| id | print real and effective user and group IDs | id USERNAME |
| last | show listing of last logged in users | last -5 ← last 5 logged in users <br/> last USER |
| lastlog | reports the most recent login of all users or of a given user | lastlog |
| umask | set file mode creation mask | umask 022 ← default 666-022=644 (rw-r--r--) |

### 네트워크

#### 네트워크
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ip | show / manipulate routing, devices, policy routing and tunnels | ip a(=addr) ← print ip address <br/> ip r(=route) ← show IP routing |
| ss | another utility to investigate sockets | ss -lt ← list all listening TCP connections <br/> ss -ua ← List all UDP connections <br/> ss -ltp ← process name with listening TCP |
| ifconfig | configure a network interface | ifconfig ← check ip <br/> ifconfig -a ← -a = display all interfaces <br/> ifconfig eth0 up or d |
| ifdown | take a network interface down | ifdown eth0 |
| ifup | bring a network interface up | ifup eth0 |
| route | show / manipulate the IP routing table | route ← show the IP routing table <br/> rout add-net 192.168.10.0 netmast 255.255.255.0 gw 10.51.0.1 |
| ethtool | Display or change ethernet card settings | ethtool eth0 <br/> ethtool -s eth0 speed 100 duplex full autoneg off |
| mii-tool | view, manipulate media-independent interface status | mii-tool eth0 |
| arp | manipulate the system ARP cache | arp -n <br/> arp -an ← a = show the entries of the specified hosts <br/> arp -d 192.168.xx.xx ← delete arp |
| tcpdump | dump traffic on a network | tcpdump -n port 80 -i any <br/> tcpdump -n not arp and not port 123 and not port 22 <br/> tcpdump -r /tmp/20210121.pcap ← -r = read packets from file |

#### 네트워크 연결 확인
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ping | send ICMP ECHO_REQUEST to network hosts |  |
| traceroute | print the route packets trace to network host | ping -c 5 -s 1500 192.168.0.1 |
| tracepath | traces path to a network host discovering MTU along this path | traceroute -n 192.168.0.10 <br/> traceroute -T -p 80 192.168.0.10 ← -T = TCP |
| nmap | Network exploration tool and security / port scanner | nmap google.com ← check TCP <br/> nmap -Pn -sT -p 22 xx.xx.xx.xx ← check firewall <br/> nmap -p 443 www.google.com |
| nc <br/> netcat | Concatenate and redirect sockets | nc 192.168.0.10 80 22 ← check TCP <br/> nc -vz 192.168.0.10 1-1023 ← port scan |
| httping | measure the latency and throughput of a webserver |  |

#### DNS
| 명령어 | 설명 | 예시 | 
|---|---|---|
| dig | DNS lookup utility | dig [@global-server] [domain] [q-type] <br/> dig @8.8.8.8 google.com any |
| nslookup | query Internet name servers interactively | nslookup -type=any google.com 8.8.8.8  |
| host | DNS lookup utility | host [-t type] [server] <br/> host -t mx gmail.com 8.8.8.8  |
| whois | client for the whois service | whois google.com |
| nscd | name service cache daemon | nscd -i hosts ← clear cache |

#### 연결
| 명령어 | 설명 | 예시 | 
|---|---|---|
| telnet | user interface to the TELNET protocol | telnet IP PORT |
| ssh | OpenSSH SSH client (remote login program) | ssh USER@IP <br/> ssh xx.xx.xx.xx sudo /sbin/reboot |
| scp | secure copy (remote file copy program) | scp -rp /tmp/test1/ user1@192.168.0.10:/tmp/test2/ |
| rsync | a fast, versatile, remote (and local) file-copying tool | rsync -avz --delete /home/user1/ /tmp/user1.bk/ ← rsync “/” is very important. <br/> rsync -e ssh -avz --delete /home/user1/ user2@192.168.0.2:/home/backup/server1/home/user1/|
| ssh-keygen | authentication key generation, management and conversion | ssh-keygen -t rsa ← generate RSA key pair <br/> ssh-keygen -R HOST  
← removes all keys belonging to hostname from a known_hosts file |
| ssh-copy-id | use locally available keys to authorise logins on a remote machine | ssh-copy-id USER@xx.xx.xx.xx |

#### HTTP
| 명령어 | 설명 | 예시 | 
|---|---|---|
| curl | transfer a URL | curl -I http://www.example.com/ ← Only Header <br/> curl -i http://www.example.com/ ← Header and Body |
| wget | The non-interactive network downloader | wget http://google.com/ <br/> wget -e http_proxy=xx.xx.xx.xx:8080 http://example.com/ |

### 하드웨어

### 하드웨어
| 명령어 | 설명 | 예시 | 
|---|---|---|
| dmesg | print or control the kernel ring buffer | dmesg |
| lsusb | List USB devices | lsusb |
| lspci | list all PCI devices | lspci |
| nproc | print the number of processing units available | nproc  |
| inxi | Display info about all hardware | inxi -Fxz |
| hwinfo | Display info about all hardware | hwinfo --short |
| lshw | Display info about all hardware  | lshw --short <br/> lshw -C cpu ← display all CPU info <br/> lshw -short -C memory <br/> lshw -short -C disk <br/> lshw -C network |
| lscpu | Display all CPU info | lscpu |

#### 모듈
| 명령어 | 설명 | 예시 | 
|---|---|---|
| lsmod | show the status of modules in the Linux Kernel | lsmod |
| modinfo | show information about a Linux Kernel module | modinfo MODULENAME <br/> modinfo bnx2 |
| insmod | insert a module into the Linux Kernel |  |
| rmmod | remove a module from the Linux Kernel |  |
| modprobe | add and remove modules from the Linux Kernel |  |

### 디스크 유틸(Utilities)

####  HDD
| 명령어 | 설명 | 예시 | 
|---|---|---|
| du | estimate file space usage | du -sh * <br/> du -sh dir/ <br/> du -h --max-depth=1 |
| fuser | identify processes using files or sockets | fuser -mv /mnt/test ← check <br/> fuser -mvk /mnt/test ← -k = kill processes |
| chroot | run command or interactive shell with special root directory |  |
| hdparm | get/set hard disk parameters |  |
| dumpe2fs | dump ext2/ext3/ext4 filesystem information |  |
| badblocks | search a device for bad blocks |  | 

#### 파티션
| 명령어 | 설명 | 예시 | 
|---|---|---|
| df | report file system disk space usage | df -h ← -h = print sizes in human readable format <br/> df -BM ← megabyte unite |
| sfdisk | partition table manipulator for Linux | sfdisk -l ← -l = list the partitions of a device |
| fdisk | manipulate disk partition table | fdisk -l ← -l = list the partition tables <br/> fdisk -l /dev/sdb |
| gdisk | Interactive GUID partition table (GPT) manipulator |  |
| parted | a partition manipulation program | parted -l ← check partitions <br/> parted /dev/mapper/mapth0 |
| lsblk | list block devices | lsblk |
| e2label | Change the label on an ext2/ext3/ext4 filesystem |  |

#### Swap
| 명령어 | 설명 | 예시 | 
|---|---|---|
| mkswap | set up a Linux swap area |  |
| swapon | enable devices and files for paging and swapping | swapon -s ← check <br/> swapon -a <br/> swapon /dev/xvda3 |
| swapoff | disable devices and files for paging and swapping | swappoff -a |

#### 파일 시스템
| 명령어 | 설명 | 예시 | 
|---|---|---|
| mkfs | build a Linux filesystem <br/> #you must umount the device before mkfs) | mkfs -t xfs /dev/sdb1 <br/> mkfs -t ext4 /dev/sdb2 |
| mkfs.xfs <br/> mkfs.ext4 <br/> mkfs.ext3 | #you must umount the device before mkfs. | mkfs.ext4 /dev/sdb1 |
| mkfs2fs | create an ext2/ext3/ext4 filesystem <br/> #you must umount the device before mkfs | mke2fs /dev/sdb1 ← ext2 |
| tune2fs | adjust tunable filesystem parameters on ext2/ext3/ext4 filesystems | tune2fs -l /dev/mapper/mpath0 ← -l = list the contents of the filesystem superblock <br/> tune2fs -i 0 -c 0 /dev/mapper/mpath0 ← -i = interval, -c = mount count |
| fsck | check and repair a Linux filesystem you must umount the device before fsck. for example single usermode and umount. ('shutdown -r -F now' is force fsck after reboot.) | fsck -p /dev/sda1 ← -p = automatically repair the file system |
| fsck.ext4 | check and repair a Linux filesystem |  |
| e2fsck | check a Linux ext2/ext3/ext4 file system |  |
| resize2fs |ext2/ext3/ext4 file system resizer | resize2fs /dev/testvg/lvol0 |

#### 데이터
| 명령어 | 설명 | 예시 | 
|---|---|---|
| dd | convert and copy a file | dd if=/dev/zero of=test_10M bs=1M count=10 |
| sync | flush file system buffers |  |
| shred | overwrite a file to hide its contents, and optionally delete it |  |

#### 마운트
| 명령어 | 설명 | 예시 | 
|---|---|---|
| mount | mount a filesystem | mount /mnt/test /dev/sda1 <br/> mount -o remount /dev/sda1 |
| umount | unmount file systems | umount /mnt/test <br/> umount -f /mnt/test ← -f = force unmount(in case of an unreachable NFS system) |

### 성능

#### 성능
| 명령어 | 설명 | 예시 | 
|---|---|---|
| top | display Linux processes | top -b -n 4 -d 5 ← interval 5 sec, 4 times |
| sar | Collect, report, or save system activity information | sar -f /var/log/sa/sa16 <br/> sar (cpu, io) <br/> sar -r (memory) |
| vmstat | Report virtual memory statistics | vmstat 1 ← interval 1sec (cpu, io, memory, swap) <br/> vmstast 1 5 ← interval 1 sec, 5 times |
| iostat | Report Central Processing Unit (CPU) statistics and input/output statistics for devices and partitions. | iostat -xtk 1 (cpu, io) ← interval 1 sec |
| mpstat | Report processors related statistics. | mpstat -P ALL |
| w | Show who is logged on and what they are doing |  |
| free | Display amount of free and used memory in the system | free -m ← show output in MB |
| netstat | Print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships | netstat -anp ← a = show both listening and non-listening sockets <br/> netstat -rn ← -r = display the kernel routing tables |
| iotop | simple top-like I/O monitor | iotop -b -n 4 -d 15 ← interval 15 sec, 4 times |
| dstat | versatile tool for generating system resource statistics | dstat -taf |
