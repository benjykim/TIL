# [CentOS] Wheel 권한 설정
 * `wheel` : 외부에서 루트(root) 사용자로 바로 로그인 하지 못하게 하고, 일반 유저로 접근 가능하게 함
 * `wheel` 그룹 : 관리자 권한을 대행하는 그룹

일반적인 리눅스 시스템에서 `su`나 `sudo`의 경우, 관리자 외의 일반계정 사용자들이 사용할 필요가 없는 명령어들이다. 
<br/>

그룹 사용자 외에는 위 명령 자체를 실행하지 못하도록 하려면, 해당 파일의 그룹을 `wheel`로 변경한 다음 `wheel` 그룹에 사용자들을 넣어주고 그 외의 계정 사용자들은 명령어 접근 자체를 제한 시켜 시스템 보안을 높일 수 있다.
 
 ### Wheel 권한 설정 예시
 1. vi 편집기로 `/etc/pam.d` 디렉터리에 있는 `su`파일을 연다.  열어보면 `auth required pam_wheel.so use_uid` 라는 부분이 주석처리되어 있는데, 이 부분을 주석 해제한다.
 2. 이후에 유저를 한 명 추가한다. (`useradd`, `passwd`)
 3. 생성한 유저를 `wheel` 그룹에 추가한다. (`vi /etc/group`으로 확인)
 4. `bin`에 있는 `su` 실행 파일을 `wheel` 그룹에 실행 권한 부여
	 ```
	 $ chgrp wheel /bin/su
	 $ chmod 4750 /bin/su
	 ```
5. 마지막으로 `root` 아이디의 로그인을 막는다. `/etc/ssh/sshd_config` 파일에서 `PermitRootLogin`을 `no`로 바꾼다.
6. 재부팅을 진행한다.

---
* 참고 : https://cyuu.tistory.com/5
