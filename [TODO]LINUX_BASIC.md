# LINUX 기초 지식
### 계정 및 그룹
#### useradd, userdel, usermod
* `useradd` : 사용자 계정 추가
	```
	$ useradd ben
	
	# 옵션
	-c : 간단한 사용자 설명 코멘트 등록
	-d : 생성하는 계정 사용자의 홈 디렉터리 위치 지정 (-d /home/ben
	-e : 생성하는 계정의 사용종료 일자 지정 (-e 2021-08-01)
	-f : 생성하는 계정의 패스워크 유효일자 지정 (-f -30, 앞으로 30일 동안 유효한 계정)
	-g : 생성하는 계정의 로그인 그룹, 지정하지 않을 경우 자동으로 사용자명과 같은 그룹 생성
	-G : 생성하는 계정의 추가등록 계정의 그룹명, 여러개일 경우 ,(콤마)로 구분. 지정하여도 사용자명과 같은 그룹은 자동으로 생성
	-p : 생성하는 계정의 패스워드 지정
	-s : 생성하는 계정의 로그인 쉘 (기본 값 /bin/bash)
	-u : 생성하는 계정의 UID 지정 (-u 1000)
	```
	* `useradd -D` : `useradd`로 생성되는 것들의 기본 값들을 설정하는 명령어
		* `/etc/default/useradd`에 이 설정 값들이 저장되어 있으며 수정 가능. `useradd -D`는 이 파일의 설정 값을 변경함
		    ```
		    $ useradd -D 
		    
		    # 출력
		    GROUP=100 
		    HOME=/home 
		    INACTIVE=-1 
		    EXPIRE= 
		    SHELL=/bin/bash 
		    SKEL=/etc/skel 
		    CREATE_MAIL_SPOOL=yes  
		    
		    # 예시
		    $ useradd -D -b /user // 홈디렉터리는 /usr로 변경
		    $ useradd -D -s /bin/sh // 기본 쉘을 /bin/sh(본쉘)로 변경
		    ```
    * 생성한 계정 확인
	    * `/etc/passwd`에서 ben의 계정 정보 확인 가능

<br/>

* `userdel` : 사용자  계정 삭제
	* `/etc/passwd` 내의 계정 내용, `/etc/shadow` 내의 패스워드 내용, `/etc/group` 파일 내의 그룹 정보 내용, 그리고 `-r`옵션을 사용하여 계정을 삭제하면  `/var/spool/mail` 디렉터리에 있는 메일 파일과 홈 디렉터리의 내용 모두를 삭제함
	* `userdel` 명령어를 사용할 땐 `-r` 옵션을 사용했을 때와 사용하지 않았을 때의 차이점을 분명히 알고 있어야 한다.
		* `userdel ben` : `/etc/passwd`, `/etc/shadow`, `/etc/group` 파일 내에 ben 에 대한 설정 값들은 모두 삭제된다. 그러나 `ls -al /home/ben`을 했을 시 ben의 홈 디렉터리와 메일 파일은 그대로 존재한다.
		* `userdel -r ben` : ben의 홈 디렉터리와 메일 파일까지 삭제된다.

<br/>

* `usermod` : 사용자 계정 정보 수정
	* `usermod` 명령어 옵션
		```
		$ usermod -U 700 ben
		
		# 옵션 
		-u uid : 새로운 UID 지정
		-g gid : 새로운 GID 지정
		-G groups : 새로운 보조그룹 지정
		-d 홈 디렉터리 : 새로운 홈 디렉터리 지정
		-s 쉘 : 새로운 쉘을 지정
		-c 주석 : 새로운 주석 지정
		-I ID : 로그인 ID를 바꾸는 옵션
		```

#### groupadd, groupdel, groupmod
* `groupadd` :  새로운 그룹 생성
	```
	$ groupadd roottest
	$ cat /etc/group | grep roottest
	roottest: x: 503: // gid를 지정하지 않았고 -r 옵션도 없기 때문에 가장 빠른 500번대 이후 숫자를 부여 받음
	
	# 옵션
	-g, --gid GID : 그룹에 gid를 지정
	-K  --key KEY=VALUE : /etc/login.defs defaults에 오버라이드
	-p, --password PASSWORD : use this encryted password for the new group
	-r, --system : 시스템에 사용되는 gid 부여 (500번 이하의 가장 빠른 gid 생성)
	```

<br/>

* `groupdel` :  그룹 삭제
	* 그룹 안에 계정이 속해 있을 경우 삭제되지 않음
     ```
     $ groupdel roottest510
     ```

<br/>

* `groupmod` : 그룹 수정
	```
	$ groupmod -g 510 -n roottest510 roottest
	// roottest라는 그룹의 gid와 그룹명 변경 (기본 gid - 503 인데, 510으로 변경 그리고 그룹명은 'roottest'에서 'roottest510'으로 변경)
	
	# 옵션
	-g, --gid GID : gid 변경 (-o 옵션과 같이 사용하여 중복 설정을 할 수도 있음)
	-n, --new-name NEW_GROUP : 그룹명을 변경할 때 사용
	```
#### /etc/passwd, /etc/shadow, /etc/group
* `/etc/passwd` : 사용자 로그인 계정, 암호화된 비밀번호 UID, 기본 GID, 사용자 계정 이름(정보), 홈 디렉터리, 로그인 쉘이 저장되어 있는 파일
	* 예시
		```
		# 형식
		사용자 이름 : 패스워드 : 사용자 ID : 그룹 ID : 사용자 계정 이름(정보) : 사용자 계정 홈 디렉터리 : 사용자 계정 로그인 쉘
		
		EX) root:x:0:0:root:/root:/bin/bash
		
		```
	* 최신 리눅스 시스템에서는 위의 예제의 패스워드처럼 `x` 라는 문자가 있는데, 이는 시스템에서 shadow 패스워드가 사용되고 있음을 나타냄

<br/>

* `/etc/shadow` : 사용자 계정에 대한 암호화된 패스워드를 저장하고 있고, 패스워드 설정 기간이나 유효성 정보도 들어 있는 파일
	* `passwd` 파일에 암호화된 패스워드를 저장하지 않고 모든 사용자들이 읽을 수 있게 하고, 대신 `shadow` 파일에 패스워드를 저장하여 루트 사용자나 특별권한이 있는 프로그램에 의해서만(ex. 로그인 프로그램) 읽을 수 있도록 함
	* 예시
		```
		# 형식
		사용자명 : 패스워드 : 패스워드 파일 최종 수정일 : 패스워드 변경 최소일 : 패스워드 변경 최대일 : 패스워드 만료 경고 기간 : 패스워드 파기 기간 : 계정 만료 기간 : 예약 필드
		root:$1$9L2L0oTwd:12751:0:99999:7 : : :
		```

<br/>

* `/etc/group` : 그룹 목록이 있으며 한 줄에 한 그룹이 표시되고, 각 그룹은 4개의 기본 항목으로 구성된다. 
	* 시스템에 접속한 사용자는 적어도 한 개의 그룹에 속한다.
	*  그룹을 추가하려면 `passwd` 파일을 열어서 기본 그룹 ID(GID)에 추가하면 된다. 
	* 예시
		```
		# 형식
		그룹 이름 : 그룹 패스워드(선택사항) : 그룹 ID : 그룹 멤버
		bin:x:1:root.bin,daemon
		```
		* 그룹 패스워드 : 소속되지 않은 사용자를 가입시킬 대 설정하여 사용 가능

#### /etc/pam.d
* `/etc/pam.d/~` : PAM 라이브러리를 이용하는 응용프로그램의 설정 파일 위치. 파일명은 서비스 이름으로 되어 있음.
* `/etc/security` : PAM 모듈 실행에 필요한 설정 파일. 파일명은 해당 서비스명.conf
* `/lib/security` : PAM 라이브러리가 제공하는 인증 모듈들과 라이브러리(*.so)로 구현되어 있음

#### /etc/login.defs
* `/etc/login.defs` : 패스워드의 사용기간 만료, 패스워드 최대 사용기간, 패스워드의 최소 변경기간 등의 패스워드 정책을 설정
* `chage` : 기존 사용자의 암호 사용 설정 변경

---

### 프로세스
#### /etc/init.d, /etc/rc.d/rc*.d
* `/etc/init.d` : 이 디렉터리는 System V init tool(SysVinit)가 사용하는 스크립트를 담고 있다. 이는 리눅스가 지금까지 전통적으로 사용해온 서비스 관리 프로그램인 init 프로세스가 사용하는 스크립트들이다. <br/> <br/>`/etc/init.d` 안에는 init 프로세스가 특정한 서비스(apache, mysql 등)들을 start, stop, restart, reload 할 수 있는 쉘 스크립트들이 들어가 있다. 이 스크립트들은 사용자가 직접 실행할 수도 있고, `/etc/rc.d/rc*.d` 디렉터리에 링크가 연결되어 부팅 시에 자동으로 실행하게 할 수도 있다.
	* `init 프로세스` : 커널 초기화 이후 가장 처음 실행되는 프로세스
* `/etc/init` : 이 디렉터리는 Upstart가 사용하는 설정 파일들을 담고 있다. Upstart는 너무 오래된 init을 대체하기 위해 최근에 만들어진 프로그램이다. `/etc/init` 디렉터리에는 Upstart가 start, stop, reload, status 명령을 통해서 특정 서비스를 어떻게 동작시켜야 하는 지에 대한 설정을 담고 있다. (Upstart는 systemd로 언젠간 대체될 예정임)

#### .d 디렉터리 이름에 대한 설명
`.d` 디렉터리 이름은 일반적으로 어떤 환경에 필요한 설정이나 스크립트를 담고 있다는 의미로 쓰인다. <br/>
예를 들어 `/etc/apt/sources.list.d`는 `sources.list`를 작성하는데 필요한 파일들이 들어 있다. `/etc/network/if-up.d`는 네트워크 인터페이스를 활성화 시킬 때 필요한 스크립트를 담고 있다.

#### /etc/inittab
* `/etc/inittab` : 리눅스 부팅 시 어떠한 방법으로 부팅할 것인지 설정할 수 있는 파일. 해당 파일을 init 프로세스가 돌면서 읽고 해당 설정에 따라 부팅을 하게 된다.
* `runlevel` 설명
	```
	0 : 시스템 종료이다. 이것으로 설정하게되면 리눅스가 시작되자마자 종료하므로 싱글부트 모드로 접속하여 값을 변경해서 복구시켜야한다. (사용금지)

	1 : 윈도우 시스템의 안전모드와 비슷하다. 싱글 부트모드로 접속하는것인데 쓸일이 별로 없을것이다.

	왜냐, 보통 싱글부트모드는 grub에서 변경해서 들어가는 방법을 사용하므로.

	2 : 네트워크가 연결 되지 않은 상태로 부팅이다.

	3 : 리눅스의 기본 모드로서 CUI 환경이다. 텍스트 모드라고 봐도 무방.

	4 : 사용자 정의 레벨 으로서 예약된곳이다. 비워져 있다.

	5 : 그래픽모드 즉 x-windows 를 부팅할때 자동으로 불러오려면 5를 사용하게 된다.

	6 : reboot , 부팅되자마자 재부팅이 된다 (사용금지)
	```

RHEL 7 이상의 경우, `systemctl` 명령어로 관리(등록/삭제)

---

### 스토리지
#### fdisk, vgscan, vgdisplay, lvscan, lvdisplay
* `fdisk -l * diskinfo /dev/rdisk/disk10` : HP-UX의 PV 디스크 정보
	* `fdisk` : 새로운 파티션 생성, 기존 파티션 삭제, 파티션 타입 결정 등의 작업 수행 가능
	* `fdisk -l` : 현재 모든 디스크의 파티션 설정 현황 볼 수 있다.
* `vgscan` : 디스크에 있는 볼륨 그룹(VG)을 검색하여 `/etc/lvmtab` 파일을 생성함. `fdisk`를 이용하여 파티션 속성을 LVM으로 지정한 후에 이 명령을 사용한다.
* `vgdisplay` : 볼륩 그룹의 속성과 정보를 보여주는 명령어 
	* `vgdisplay -v vgdata` : 좀 더 자세히 보여주는 옵션으로 VG 이외에 LV(Logical Volume)와 PV(Physical Volume)도 같이 보여준다.
* `lvscan` : 디스크에 있는 LV를 찾아준다.
* `lvdisplay -v` : LV의 정보를 보여준다.
* `mkfs.xfs /dev/vgdata/lvdata` : `/dev/vgdata/lvdata`파티션을 XFS 파일시스템으로 포맷한다.

#### /etc/fstab, mount, umount, fuser
* `/etc/fstab` : 파일 시스템 정보를 저장하고 있으며, 리눅스 부팅 시 마운트 정보를 저장하고 있는 파일이다. 
* `mount -a` : `fstab`에 있는 모든 파일 시스템을 마운트한다.
* `mount /dev/vgdata/lvdata /mnt` : 포맷한 볼륨을 `/mnt` 에 마운트
* `mount /data` : `/data` 폴더 마운트
* `umount /data` : `/data` 폴더 언마운트
* `fuser -cu /data` : `/data` 폴더 접근하고 있는 프로세스 조회
* `fuser -cku /data` : `/data` 폴더 접근하고 있는 프로세스 강제 Kill 

---

### 네트워크
#### /etc/sysconfig/network, /etc/sysconfig/network-scripts/ifcfg-eth0
* `/etc/sysconfig/network` : 시스템 전체에 대한 글로벌한 기본 게이트웨이 주소 설정과 호스트네임, 네트워킹 연결 허용 여부를 설정하는 파일 (호스트네임은 설치 중에 입력한 내용이 반영됨)
	* 예시
		```
		NETWORKING=yes
		HOSTNAME=benjykim.com
		GATEWAY=192.168.0.1
		```
		설정 변경 후에는 `/etc/rc.d/init.d/network restart`로 네트워크를 재시작한다.
	`/etc/sysconfig/network-scripts/ifcfg-eth0` : `eth0`은 시스템에 설치되어 있는 이더넷 카드(랜카드)의 명칭으로 첫 번째 이더넷 카드를 뜻한다. 이 설정 파일에서 게이트 웨이 및 다른 설정사항들이 다른 설정파일과 중복될 수 있는데, 이러한 경우 이곳의 설정 파일이 우선시되어 적용된다.
		예시
		```
		vi /etc/sysconfig/network-scripts/ifcfg-eth0
		
		# 장치명, 첫번째 이더넷카드 
		DEVICE=eth0 
		# IP 부여 방식 결정, static 은 고정IP 
		BOOTPROTO=static 
		# 이더넷카드의 MAC 주소 
		HWADDR=XX:XX:XX:XX:XX:XX 
		# GUI 모드에서의 편리한 네트워크설정 허용, TUI에선 필요없음 
		NM_CONTROLLED=no 
		# 시스템 시작시 자동으로 활성화 
		ONBOOT=yes 
		# Ethernet 에 대한 설정 
		TYPE=Ethernet 
		# 고유ID를 부여하는 것으로 자동으로 부여됨 
		UUID=XXXXXXX-XXX-XXX-XXX-XXXXXXX 
		# 브로드캐스트 지정 
		BROADCAST=192.168.0.255 
		# IP 주소 지정 
		IPADDR=192.168.0.5 
		# 서브넷마스크 지정 
		NETMASK=255.255.255.0 
		# 네트워크 지정 
		NETWORK=192.168.0.0 
		# Wake On Lan 기능 활성화, Ethtool 이 필요한데 CentOS 기본 설치되어 있음 
		ETHTOOL_OPTS=wol g 
		# 일반사용자의 eth0 제어 가능여부 
		USERCTL=no 
		# IPV6 사용여부 
		IPV6INIT=no  
		```

* `/etc/sysconfig/network-scripts/ifcfg-XXX`

#### ifconfig, netstat 
* `ifconfig` : "interface configuration"의 약자로 리눅스의 네트워크 관리를 위한 인터페이스 구성 유틸리티이다. <br/>
 `ifconfig` 명령은 현재 네트워크 구성 정보를 표시하고, 네트워크 인터페이스에 IP 주소, 넷 마스크 또는 Broadcast 주소를 설정하고, 네트워크 인터페이스의 별칭을 만들고, 하드웨어 주소를 설정하고, 네트워크 인터페이스를 활성화/비활성화하는 등 다양한 곳에서 사용된다. 
	* `ifconfig -a` : 모든 네트워크 인터페이스 구성 확인(비활성화된 네트워크 인터페이스도 볼 수 있음)
	* `ifconfig [interface]` : 인터페이스가 많은 경우 원하는 인터페이스만 볼 수 있다.
		```
		$ ifconfig enp0s4
		```
	* `ifconfig [interface] down` : 해당 인터페이스 비활성화
	* `ifconfig enp0s4 up` : 해당 인터페이스 활성화
	* `ifconfig [interface] [IP]` : 해당 인터페이스 IP 변경
		```
		$ ifconfig enp0s4 10.0.2.1
		```
	* `ifconfig [interface] netmask [IP]` : 해당 인터페이스 넷마스크 변경
		```
		$ ifconfig enp0s4 netamsk 255.255.0.0
		```
	* `ifconfig [interface] broadcast [IP]` : 해당 인터페이스 브로드캐스트 주소 변경
		```
		$ ifconfig enp0s4 broadcast 10.0.255.255
		```
	* `ifconfig [interface] [IP] netmask [IP] broadcast [IP]` : 해당 인터페이스 IP/넷마스크/브로드캐스트 주소 한꺼번에 변경
		```
		$ ifconfig enp0s4 10.0.2.1 netmask 255.255.0.0 broadcast 10.0.255.255
		```
	* `ifconfig [interface] hw ether [mac address] `: 해당 인터페이스 맥 어드레스 할당
		```
		$ ifconfig enp0s4 08:00:27:54:10:f1
		```
   * `ifconfig [interface]:0 [IP]` : 인터페이스에 별칭 추가해서 가상 인터페이스 만들기
	   ```
	   $ ifconfig enp0s4:0 10.0.2.10
	   ```
* `netstat` : 네트워크 연결 상태, 라우팅 테이블, 인터페이스 상태 등을 보여주는 명령어
	* `netstat -an` : 모든 네트워크의 상태를 출력(`-a` 옵션), 도메인 숫자를 숫자로 출력(`-n` 옵션)
	* `netstat -rn` : 라우팅 테이블 출력(`-r` 옵션)

#### tcpdump
* `tcpdump -i ens192` : 인터페이스 ens192 를 보여준다.
* `tcpdump -i ens192 host 10.0.0.1` : host를 지정하면, 이 IP 로 들어오거나 나가는 양방향 패킷을 모두 보여준다.
* `tcpdump -i ens192 port 22` : 포트 양방향으로 22인 것에 대해 보여준다. 
* `tcpdump -i ens192 host 10.0.0.1 and port 22 -nn` : `-nn` 옵션을 사용하면 프로토콜과 포트번호를 이름으로 바꾸지 않는다. (이 옵션을 사용하지 않으면 포트 번호와 프로토콜이 숫자로 나오지 않고 이름으로 나오게 된다.)

---

### 커널 파라미터 및 TCP 파라미터
* `cat /proc/sys/net/ipv4/tcp_keepalive_time` : `keepalive` 소켓의 유지시간. 타이머는 이 시간을 기준으로 동작하며 이 시간이 지나면 `keepalive` 확인 패킷을 보낸다.
* `vi /etc/security/limits.conf` : nproc, nofile 설정
	* `nofile` : 해당 도메인(사용자, 그룹)이 오픈할 수 있는 최대 파일 개수
	* `nproc` : 해당 도메인(사용자, 그룹)의 최대 프로세스 개수

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

* 참고
	* https://webdir.tistory.com/128
	* https://www.linux.co.kr/home/lecture/?leccode=216
	* https://rootblog.tistory.com/2
	* https://webdir.tistory.com/129
	* https://seoulforest.tistory.com/entry/%EB%A6%AC%EB%88%85%EC%8A%A4-study-1
	* https://hschang3.tistory.com/4
	* https://helloitstory.tistory.com/25
	* https://3sikkim.tistory.com/7
	* https://itdexter.tistory.com/311
	* https://webdir.tistory.com/116
	* https://www.whatap.io/ko/blog/11/
