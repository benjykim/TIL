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
| NFS status | `lssrc -a | grep -i nfs` |
| NFS reload | `exportfs -av` |
| NFS performance | `nfsstat` |
| NFS Options | `nfso -a` / `nfso -o <option>=<value>` |

### NTP
| 구분 | 명령어 |
|---|---|
| Time daemons | `xntpd` |
| NTP setup | `/etc/ntp.conf` / `startsrc -s xntpd` / `stopsrc -s xntpd` / `lslpp -L all | grep xntpd`|
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
