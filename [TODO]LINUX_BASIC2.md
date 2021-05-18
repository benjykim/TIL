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
* `groupdel` :  그룹 삭제
	* 그룹 안에 계정이 속해 있을 경우 삭제되지 않음
     ```
     $ groupdel roottest510
     ```
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
* `/etc/shadow` : 사용자 계정에 대한 암호화된 패스워드를 저장하고 있고, 패스워드 설정 기간이나 유효성 정보도 들어 있는 파일
	* `passwd` 파일에 암호화된 패스워드를 저장하지 않고 모든 사용자들이 읽을 수 있게 하고, 대신 `shadow` 파일에 패스워드를 저장하여 루트 사용자나 특별권한이 있는 프로그램에 의해서만(ex. 로그인 프로그램) 읽을 수 있도록 함
	* 예시
		```
		# 형식
		사용자명 : 패스워드 : 패스워드 파일 최종 수정일 : 패스워드 변경 최소일 : 패스워드 변경 최대일 : 패스워드 만료 경고 기간 : 패스워드 파기 기간 : 계정 만료 기간 : 예약 필드
		root:$1$9L2L0oTwd:12751:0:99999:7 : : :
		```
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
