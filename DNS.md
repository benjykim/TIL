# DNS
`DNS`란 도메인을 IP로 변환하거나 도메인으로 다시 변경해주는 것을 말한다. DNS가 없다면 우리가 외우고 있는 `naver.com`등은 소용이 없고 `naver.com`의 IP 주소를 외우고 다녔어야 한다.
</br>

### 역할
1. 호스트 별칭
	* 복잡한 호스트 이름을 가진 호스트는 하나 이상의 별명을 가질 수 있다. 예를 들어, `relay1.west-coast.enterprise.com` 이라는 호스트는 `enterprise.com`과 `www.enterprise.com` 같은 2개의 별칭을 가질 수 있다.
2. 메일서버 별칭
3. 부하 분산
	* 같은 도메인에 대해서 여러 IP 지정이 가능하다. 따라서 같은 도메인에 대하여 사용자가 질의를 할 경우, DNS 서버는 IP를 번갈아가며 알려준다.
</br>

### 특징
* HTTP, FTP, SMTP 등과 달리 사용자가 직접 요청하는 프로토콜이 아니다. 
	* 예를 들어, 사용자가 브라우저에서 `naver.com`을 검색하는 경우, 브라우저가 DNS 서버에게 `naver.com`의 IP로 변경해달라고 요청한다.

</br>

### 포트
* UDP/53 --> 평소에 사용한다.
* TCP/53 --> 딱 두 가지 경우에만 TCP 사용
	1. 전송 데이터가 512 Byte 이상일 때
	2. Zone transfer (존 영역을 전송하는 경우)

</br>

### 구조
1. ROOT DNS 서버
2. 최상위 도메인 DNS 서버
3. 책임 DNS 서버

</br>

### 동작과정
`naver.com`을 찾는다고 가정하고 진행한다. (그림 생략)
1. DNS Client(웹 브라우저 등) 로컬 DNS에게 `www.naver.com`을 질의한다.
2. 로컬 DNS가 Root DNS에게 `www.naver.com`을 묻는다.
3. Root DNS가 `com`을 인식하고 `com`을 관리하는 최상위 도메인 DNS 서버의 IP를 로컬 DNS에게 알려준다.
4. 다시 로컬 DNS가 `com`도메인을 관리하는 최상위 도메인 DNS 서버에게 `www.naver.com`을 질의한다.
5. 최상위 도메인 DNS 서버가 `naver.com`을 인식하고 `naver.com`을 관리하는 책임 DNS 서버의 IP를 로컬 DNS에게 알려준다.
6. 로컬 DNS는 책임 DNS 서버에게 `www.naver.com`을 질의한다.
7. 책임 DNS 서버에서 `www.naver.com` 호스트에 대한 IP를 알려준다.
8. 로컬 DNS가 DNS Client 에게 `www.naver.com`의 IP 주소를 알려준다.

</br>

### DNS 캐싱
Local DNS는 질의에 대해서 DNS Server로 부터 응답을 받을 때 마다 DNS 정보(호스트 이름과 IP 주소 쌍)를 저장한다. 이 때 다시 클라이언트로부터 같은 질의가 Local DNS에 도착한다면 해당 도메인에 대한 책임이 없더라도 해당 호스트 이름에 대한 IP를 응답한다.

</br>

### 시리얼 번호
DNS 영역 파일에 레코드를 추가하거나 삭제를 하면 시리얼 번호가 증가하게 된다. Master DNS, Slave DNS 가 있을 때 이 시리얼 번호를 확인하고 자신보다 높은 숫자라면 최신 정보로 인지하여 해당 DNS로 부터 레코드들을 받아온다.

</br>

### 명령어
* 캐시 삭제: `ipconfig /flushdns`
* 캐시 확인: `ipconfig /displaydns`

</br>

### 레코드와 메시지
* 각 DNS 서버들은 호스트 이름은 IP 주소로 맵핑하기 위한 자원 레코드를 저장한다.
* DNS는 요청에 대해 하나 이상의 레코드를 가진 메시지로 응답한다.

</br>

#### 자원 레코드 구조 - Name, Value, Type, TTL
1. Type = A: Name은 HostName, Value는 호스트 네임에 대한 IP 주소
	* 예) `naver.com`, 145.34.1.2, A
	* DNS 서버가 어떤 호스트 네임에 대한 책임 DNS 서버이면, 그 호스트 네임에 대한 Type A 레코드를 포함한다.
2. Type = NS: Name은 도메인, Value는 도메인 내부의 호스트에 대한 IP 를 얻을 수 있는 방법을 아는 책임 DNS 서버의 호스트 네임
	* 예) `naver.com`, `dns.naver.com`, NS
	* 서버가 호스트 네임에 대한 책임 서버가 아니라면, 그 서버는 호스트 네임을 포함하는 도메인에 대한 Type NS 레코드를 포함
3. Type = CNAME: Name은 별칭 HostName, Value는 정식 HostName
	* 예) `naver.com`, `relay1.bar.naver.com`, CNAME
4. Type = MX: Name은 별칭 HostName, Value는 정식 메일서버 네임
	* 예) `naver.com`, `mail.naver.com`, MX

---
* 출처: https://galid1.tistory.com/53
