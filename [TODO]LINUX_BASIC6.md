### 커널 파라미터 및 TCP 파라미터
* `cat /proc/sys/net/ipv4/tcp_keepalive_time` : TCP 
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
