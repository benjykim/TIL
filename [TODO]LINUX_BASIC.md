# LINUX 기초 지식
### 계정 및 그룹
* `useradd` : 
* `userdel` : 
* `usermod` : 
* `groupadd` : 
* `groupdel` : 
* `groupmod` :
<br/> 
* `/etc/passwd` : 
* `/etc/shadow` : 
* `/etc/group` : 
* `/etc/pam.d/~` : 
* `/etc/login.defs` : 

---

### 프로세스
* `/etc/init.d` : 
* `/etc/rc.d/rc*.d` : 
* `/etc/inittab` : 

RHEL 7 이상의 경우, `systemctl` 명령어로 관리(등록/삭제)

---


### 스토리지
* `fdisk -l * diskinfo /dev/rdisk/disk10` : 
* `vgscan` : 
* `vgdisplay -v vgdata` : 
* `lvscan` : 
* `lvdisplay -v`:
* `mkfs.xfs /dev/vgdata/lvdata` : 
* `vi /etc/fstab` : 
* `mount -a` :
* `mount /dev/vgdata/lvdata` : 
* `mount /data` : 
* `umount /data` : 
* `fuser -cu /data` : 
* `fuser -cku /data` : 
---


### 네트워크
* `cd /etc/sysconfig/network-scripts/ifcfg-XXX` : 
* `ifconfig` : 
* `netstat -an` : 
* `netstat -rn` : 
<br/>
* `tcpdump -i ens192` : 
* `tcpdump -i ens192 host 10.0.0.1` : 
* `tcpdump -i ens192 port 22` : 
* `tcpdump -i ens192 host 10.0.0.1 and port 22 -nn` :  

---


### 커널 파라미터 및 TCP 파라미터
* `cat /proc/sys/net/ipv4/tcp_keepalive_time` : TCP 
* `vi /etc/security/limits.conf` : nproc, nofile 설정

---


### 모니터링
* LINUX : `top`, `nmon`
* HP-UX : `glance`
* AIX : `nmon`

---

### HA(High Availability, 고가용성)
* HA 솔루션 = 이중화 솔루션
#### HA 솔루션 종류
* HP-UX OS의 MC/SG(=Service Guard) : HP-UX OS 이중화 솔루션
* AIX OS의 HACMP : AIX OS 이중화 솔루션)
* Veritas Infoscale : 3rd party 솔루션(외산)
* Mactech MCCS : 3rd party 솔루션(국내)

#### HA 솔루션이 하는 일
* 모니터링 : HA를 구성하는 서버들 헬스 체크하다 누군가 죽으면, 죽은 서버가 하는 역할을 다른 서버가 할 수 있도록 전환
* 디스크 미러링(=디스크 복제)
* 네트워크 VIP 전환
* 특정 서버에서 수행중인 프로세스를 해당 서버가 죽었을 때 다른 서버에서 자동으로 기동 해줌(프로세스 기동 순서, Config 파일 등등)

---

