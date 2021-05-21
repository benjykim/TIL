# STP
## STP(Spanning Tree Protocol)
* `STP` : 이더넷 프레임 장비들에서 빙빙 도는 것을 Looping(루핑)이라고 하는데, 이 Looping을 방지시켜주는 것이 STP이다.
	* L3 계층에서 사용하는 IP 패킷은 헤더에 TTL 필드가 있어 패킷의 Looping을 막아준다.

#### Spanning Tree Protocol 동작 방식
STP가 동작하면 물리적으로 Loop 구조인 네트워크에서 특정 포트를 차단 상태로 바꾸어 놓음으로써 논리적으로 Loop가 발생하지 않게 한다.

<br/>

그러다 다른 동작중인 스위치의 포트가 다운되면 차단 상태로 바꿔놓은 특정 포트를 다시 전송 상태로 바꾸어 통신이 끊기지 않도록 한다. 그 Loop를 막는 경로를 구성하는 프레임을 `BPDU(Bridge Protocol Data Unit)`라고 한다.

---

* 출처 : https://gmldbd94.tistory.com/104
