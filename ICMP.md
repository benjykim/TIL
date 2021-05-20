# ICMP
### ICMP(Internet Control Message Protocol)
* `ICMP` : 인터넷 제어 메시지 프로토콜

OSI 7계층 중 3계층(네트워크 레이어)의 IP 프로토콜은 전송 상태에 대한 관리가 이루어지지 않는 신뢰할 수 없는 프로토콜이다. 이러한 IP 프로토콜의 단점을 보완하기 위한 프로토콜이 `ICMP` 프로토콜이다. 
<br/>
IP 패킷 전송 중 에러 발생 시 에러 발생 원인을 알려주거나 네트워크 상태를 진단해주는 기능을 제공함

* `Error-Reporting Message` : 전송 중 오류 발생 시 에러 메시지를 생성하여 응답

* `Query Message` : 네트워크 상태를 진단하기 위한 쿼리 요청 및 응답메시지 생성

---

ICMP 메시지는 메시지의 유형을 의미하는 `Type` 필드와 유형별 세부 내용을 담고 있는 `Code` 필드로 구성된다.
* `Type` : ICMP 메시지의 유형 / 용도
	* ex) Type 3 : Destination Unreachable (목적지 도달 불가)
* `Code` : Type의 세부 내용으로 Type과 Code가 조합되어 ICMP 메시지의 목적과 용도를 나타냄 (Code가 없는 Type도 존재)
	* ex) Type 3 Code 3 : Port Unreachable UDP (포트가 열려있지 않음)
* `Checksum` : ICMP메시지 오류를 검사하기 위한 값
* `Rest of the header` : Type과 Code에 따라 추가되는 헤더
*` Data Section` : 데이터가 위치하는 영역

---

### 주요 ICMP Error Reporting 메시지

#### Destination Unreachable (Type 3)
* 해당 목적지에 도달할 수 없음을 의미
* 목적지 도달 불가 사유에 따라 다양한 Code로 구성됨
	```
	Code 1(Host Unreachable)  : 최종 단계의 라우터가 목적지 호스트로 패킷 전송에 실패한 경우

	Code 2(Protocol Unreachable)  : 목적지 호스트에서 특정 프로토콜을 사용할 수 없는 경우

	Code 3(Port Unreachable)  : 목적지 호스트에 해당 UDP포트가 열려있지 않는 경우

	(TCP의 경우에 포트가 열려있지 않으면 TCP RST 패킷을 반환한다)

	Code 4(Fragmentation needed and don't fragment was set)  : IP 패킷의 단편화가 반드시 필요하지만 IP 헤더의 Don't fragment 플래그가 설정되어 단편화할 수 없는 경우 라우터에 의해 반환된다.
	```

#### Redirection (Type 5)
* 라우팅 경로가 잘못되어 새로운 경로를 이전 경유지 또는 호스트에게 알려주는 메시지
* ICMP Redirect 공격 시 이용하는 메시지
> 
#### Time Exceeded (Type 11)
* 타임아웃이 발생하여 IP패킷이 폐기되었음을 알리는 메시지이며 타임아웃 사유는 Code를 통해 알 수 있다.
	```
	Code 0(Time To Live exceeded in Transit)  : IP 패킷이 최종 목적지에 도달하기 전에 TTL값이 0이 되어 해당 패킷이 폐기되었음을 알리는 메시지이다.

	Code 1(Fragment reassembly time exceeded)  : IP 패킷 재조합 과정에서 타임아웃이 발생하여 해당 IP 데이터그램이 모두 폐기되었음을 알리는 메시지. 일반적으로 IP 데이터그램의 일부 단편이 전송과정에서 손실될 경우 재조합 과정에서 실패하여 발생한다.
	```
---

### 주요 ICMP Error Reporting 메시지

#### Echo Request(Type 8) Echo Reply(Type 0)

* ping 테스트에 사용되는 메시지로 End노드 간에 네트워크 및 호스트 상태진단을 목적으로 사용
* 별도의 Code는 없으며 이외의 쿼리 타입들은 거의 사용되지 않음

---

* 출처 : https://itragdoll.tistory.com/47

