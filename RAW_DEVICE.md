# Raw Device
## Raw Device란?
* `Raw Device` : 블록 장치로 구성되는 것이 아닌 문자 장치로 구성되는 방식으로, 포맷을 지정하지 않고 디스크를 구성하는 방식
*  `Raw Device 다른 설명` : 파일 시스템이 Set-up 되지 않은 디스크 드라이브
	* 문자 장치 : 문자 단위로 입출력이 이뤄지며, 커널이 제공하는 버퍼를 사용하지 않고 입출력 장치의 버퍼 또는 큐를 사용
* `Raw Device`는 커널을 거치지 않고 바로 버퍼 캐시에 접근하는 디스크 방식이어서, 파일 시스템 디스크보다 속도에서 이점을 갖음

### 등장 배경
* Raw Device의 경우 주로 DBMS의 데이터를 저장하는 공간으로 사용
* DBMS의 저장 공간으로써 가장 중요한 부분은 Disk의 I/O 성능
* Raw Device의 경우 운영체제가 필요 없으므로, File System에서 사용하는 운영체제를 거치지 않고 바로 데이터 I/O가 일어나기 때문에 뛰어난 I/O 성능을 자랑함

### 장/단점
* 장점
	* Disk I/O 성능이 우수하고, 불필요한 오버헤드가 발생하지 않음
	* OS 버퍼 크기를 줄일 수 있음
* 단점
	* 구성 및 관리가 어려움
	* OS에서 cylinder 0을 보호하지 못한다(?)
	* DBMS에서 사용 시, 데이터 파일에서 사용할 용량을 초기에 예상하고 미리 볼륨을 잡아야 하기 때문에 단편화가 발생할 수 있음
	* DBMS에서 데이터 파일 백업 시 볼륨 전체를 백업해야 하기 때문에 백업하는데 시간도 오래 걸리고 불필요한 용량을 차지함

### Raw Device를 이용한 기술
* Oracle의 RAC(Real Application Cluster)
	* Oracle DBMS 10g의 ASM(Automatic Storage Management)라는 기술이 나오기 전에 이중화와 HA를 위해 DB서버를 클러스터링 하는 기술
* Tibero의 TAC(Tibero Active Cluster)
	* 확장성, 고가용성을 목적으로 제공하는 기능으로써 TAC 환경에서 실행 중인 모든 인스턴스는 클러스터링하여 데이터베이스를 공유함
---
* 출처 : https://goldsony.tistory.com/33
