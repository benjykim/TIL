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
