
### 소프트웨어
| 조작(Operation) | 명령어 |
|---|---|
| 소프트웨어 나열(list) | `swlist` |
| HP-UX 버전 | `swlist | grep HPUX11i` |
| HP-UX pach 버전 | `swlist -l bundle QPKBASE` / `swlist -l bundle | grep QPK` |
| 소프트웨어 설치 | `swinstall -s ./SOURCE.depot` / `swinstall -s ./SOURCE.depot SOFTWARE` |
|  | `installed /usr/local` |
| 소프트웨어 제거 | `swremove ← GUI` / `swremove SOFTWARE ← CLI` |

### Operation
| 조작(Operation) | 명령어 |
|---|---|
| ls | `ls -l` |
| sudo | `visudo` |
| 현재 성능 정보 | `top` / `glance` |
| 메모리 정보 | `swapinfo` |
| 부하 평균 | `w` |
| 현재 로그인 정보 | `w` |
| 이전 로그인 정보 | `last` |

### 서비스
| 조작(Operation) | 명령어 |
|---|---|
| 서비스 중지 | `/sbin/init.d/SERVICE top` |
| 서비스 시작 | `/sbin/init.d/SERVICE start` |
