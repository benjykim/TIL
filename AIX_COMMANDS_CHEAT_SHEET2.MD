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
| Network | `[ent|tok|fddi|atm]stat` / `netstat` / `netpmon (trcstop to stop trace)` |
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
