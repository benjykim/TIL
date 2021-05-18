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
* `userdel` : 사용자  계정 삭제
	* `/etc/passwd` 내의 계정 내용, `/etc/shadow` 내의 패스워드 내용, `/etc/group` 파일 내의 그룹 정보 내용, 그리고 `-r`옵션을 사용하여 계정을 삭제하면  `/var/spool/mail` 디렉터리에 있는 메일 파일과 홈 디렉터리의 내용 모두를 삭제함
	* `userdel` 명령어를 사용할 땐 `-r` 옵션을 사용했을 때와 사용하지 않았을 때의 차이점을 분명히 알고 있어야 한다.
		* `userdel ben` : `/etc/passwd`, `/etc/shadow`, `/etc/group` 파일 내에 ben 에 대한 설정 값들은 모두 삭제된다. 그러나 `ls -al /home/ben`을 했을 시 ben의 홈 디렉터리와 메일 파일은 그대로 존재한다.
		* `userdel -r ben` : ben의 홈 디렉터리와 메일 파일까지 삭제된다.
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
