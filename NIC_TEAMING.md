# NIC Teaming
### Windows NIC Teaming
* `NIC Teaming` : 여러 개의 NIC를 묶어서 하나로 동작하게 하는 기술로, 해당 기능을 통해 네트워크 통신 속도를 높이거나 회선 장애에 대한 대비를 할 수 있다.
	* 만일 2개의 NIC를 Teaming 한 다음 하나의 NIC를 다운시킨다면, 대역폭은 줄지만 Team NIC는 여전히 활성화된 상태이고 정상적으로 네트워크가 작동하는 것을 확인할 수 있다.
	* 위와 같이, 해당 기능을 이용하면 속도 개선 및 포트 장애 또는 회선 장애에 의한 네트워크 차단 문제를 대비할 수 있다.
		* 단, NIC Teaming 으로 대역폭을 늘려 속도 개선 효과를 보기 위해서는 추가적인 스위치 셋팅이 필요하다.

### Linux, HP-UX, AIX 에서의 Teaming
* Linux : Bonding
* HP-UX : APA (Auto-Port Aggregation)
* AIX : EtherChannel
