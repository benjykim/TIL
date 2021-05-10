# SDS(Software Defined Storage)
### SDS 개요
* 기존 스토리지 인프라에서 제어 및 관리 소프트웨어를 분리하여 스토리지 리소스를 가상화한 기술
* 스토리지 리소스의 가상화로 물리 자원의 통합 및 단일화 Pool 을 구성하여 스토리지 네트워크 생성 가능
* 정형 데이터에 최적화된 Legacy 아키텍처(SAN, NAS) 대비 폭발적으로 증가하는 비정형 데이터 수요 대응 가능

### SDS 솔루션 요건
* 필수 Feature
	
	| 구분 | 내용 |
	|--|--|
	| 1) Automation | ㆍ스토리지 서비스 Deployment 및 Provisioning 과정의 자동화 <br/> ㆍStandard Interface 제공 <br/> 　(SDDC 플랫폼과의 연동을 위한 Programmable API) |
	| 2) Scalability | ㆍWeb-Scale 방식의 분산형 아키텍처 |
	| 3) Cost Effective | ㆍ범용 x86 서버 기반 <br/> ㆍOpen Source S/W 기반 (특정 벤더 종속성 탈피) |
	| 4) Monitoring | ㆍ중앙 모니터링 (구성, 성능, 변경, 장애관리) |
	| 5) Metering & Billing | ㆍ사용량 기반 과금 (Pay-as-you-go) <br/> 　(기간별 사용량 API 조회 -> 정산/과금) |

#### 기타 고려사항
* 안정성 : 실 서비스 환경에서 사용 가능한 수준의 안정성 검증 필요
* 성능 : Legacy 아키텍처(Mid-range 급) 대비 동등한 수준의 성능 보장
* S/W 지속성 : 벤더사 혹은 오픈 소스 커뮤니티의 규모 및 지속 가능성

### SDS 솔루션 유형별 제품 비교

| SDS 솔루션 유형 | 내용 |
|--|--|
| 1) Object Storage Type | ㆍ제품 : Veritas ACCESS, Hitachi HCP, DellEMC ECS 등 <br/> 　(단일 스토리지 H/W + Object Storage 솔루션) |
| 2) Storage Virtualization Type | ㆍ제품 : DellEMC VPLEX, NetApp FlexArray 등 <br/> 　(이기종 스토리지 H/W 통합 가상화 솔루션) |
| 3) HCI Type | ㆍ제품 : HPE Simplivity, Nutanix, DellEMC VxRAIL 등 <br/> 　(Server + Storage + Network 통합 가상화 솔루션) |
| 4) Commodity x86 Type | ㆍ제품 : DellEMC VxFLEX, (Open Source) CEPH, Gluster 등 <br/> 　(범용 x86 H/W + 분산 아키텍처 + Object Storage 솔루션) |


