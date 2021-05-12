# LINUX (RHEL/CENTOS) 명령어 정리
### 목차

* 시스템 명령어
	* 시스템 명령어
	* 프로세스 관리(Management)
	* 스케줄
	* ETC
* 파일 및 디렉토리 명령어
	* 파일 명령어
	* 디렉터리 명령어
	* 파일 내용에 접근하는 명령어
	* 검색
* 유저 관리(Administration)
	* 유저
	* 그룹
	* 파일 권한
	* ETC
* 네트워크
	* 네트워크
	* 네트워크 연결 확인
	* DNS
	* 연결(Connection)
	* HTTP
	* SNMP
* 하드웨어
	* 하드웨어
	* 모듈
* 디스크 유틸(Utilities)
	* HDD
	* 파티션
	* Swap
	* 파일 시스템
	* 데이터
	* 마운트
* 성능
	* 성능

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

#### 프로세스 관리(Management)
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


#### ETC
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

#### 파일 내용에 접근하는  명령어
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
| useradd | create a new user or update default new user information |  |
| adduser | add a user to the system |  |
| whoami | print effective userid |  |
| w | Show who is logged on and what they are doing |  |
| who | show who is logged on |  |
| userdel | delete a user account and related files |  |
| vipw | edit the password, group, shadow-password or shadow-group file |  |
| passwd | change user password |  |
| chpasswd | update passwords in batch mode |  |
| chage | change user password expiry information |  |
| usermod | modify a user account |  |
| gpasswd |  |  |
| chsh | change login shell |  |
| getent | get entries from Name Service Switch libraries |  |
| pam_tally2 | The login counter (tallying) module |  |

#### 그룹
| 명령어 | 설명 | 예시 | 
|---|---|---|
| groups | print the groups a user is in |  |
| groupadd | create a new group |  |
| addgroup | add group to the system |  |
| groupdel | delete a group |  |
| groupmod | change USER's GID |  |
| chgrp | change the Group of the file |  |
| vigr | edit the password, group, shadow-password or shadow-group file |  |

#### 파일 권한
| 명령어 | 설명 | 예시 | 
|---|---|---|
| chmod | change file mode bits |  |
| chown | change file owner and group |  |

#### ETC
| 명령어 | 설명 | 예시 | 
|---|---|---|
| finger | user information lookup program |  |
| su | change user ID or become superuser |  |
| sudo | execute a command as another user |  |
| id | print real and effective user and group IDs |  |
| last | show listing of last logged in users |  |
| lastlog | reports the most recent login of all users or of a given user |  |
| umask | set file mode creation mask |  |

### 네트워크

#### 네트워크
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ip | show / manipulate routing, devices, policy routing and tunnels |  |
| ss | another utility to investigate sockets |  |
| ifconfig | configure a network interface |  |
| ifdown | take a network interface down |  |
| ifup | bring a network interface up |  |
| route | show / manipulate the IP routing table |  |
| ethtool | Display or change ethernet card settings |  |
| mii-tool | view, manipulate media-independent interface status |  |
| arp | manipulate the system ARP cache |  |
| nmcli | line tool for controlling NetworkManager |  |
| tcpdump | dump traffic on a network |  |

#### 네트워크 연결 확인
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ping | send ICMP ECHO_REQUEST to network hosts |  |
| traceroute | print the route packets trace to network host |  |
| tracepath | traces path to a network host discovering MTU along this path |  |
| mtr | a network diagnostic tool |  |
| nmap | Network exploration tool and security / port scanner |  |
| nc <br/> netcat | Concatenate and redirect sockets |  |
| httping | measure the latency and throughput of a webserver |  |

#### DNS
| 명령어 | 설명 | 예시 | 
|---|---|---|
| dig | DNS lookup utility |  |
| nslookup | query Internet name servers interactively |  |
| host | DNS lookup utility |  |
| whois | client for the whois service |  |
| nscd | name service cache daemon |  |

#### Connection
| 명령어 | 설명 | 예시 | 
|---|---|---|
| telnet | user interface to the TELNET protocol |  |
| ssh | OpenSSH SSH client (remote login program) |  |
| scp | secure copy (remote file copy program) |  |
| rsync | a fast, versatile, remote (and local) file-copying tool |  |
| ssh-keygen | authentication key generation, management and conversion |  |
| ssh-copy-id | use locally available keys to authorise logins on a remote machine |  |

#### HTTP
| 명령어 | 설명 | 예시 | 
|---|---|---|
| curl | transfer a URL |  |
| wget | The non-interactive network downloader | |


#### SNMP

| 명령어 | 설명 | 예시 | 
|---|---|---|
| snmpwalk | retrieve a subtree of management values using SNMP GETNEXT requests |  |
| snmpget | communicates with a network entity using SNMP GET requests |  |
|snmptranslate  | translate MIB OID names between numeric and textual forms |  |
| snmpnetstat | display networking status and configuration information from a network entity via SNMP |  |

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
