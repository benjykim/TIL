
# HP-UX 명령어 정리

### 목차

* [하드웨어](#하드웨어)
* [OS 정보](#os-정보)
* [네트워크](#네트워크)
* [유저](#유저)
* [디스크](#디스크)
* [스토리지](#스토리지)
* [소프트웨어](#소프트웨어)
* [Operation](#operation)
* [서비스](#서비스)

---

### 하드웨어
| 조작(Operation) | 명령어 |
|---|---|
| 하드웨어 확인(CPU, Memory, Firm, OS) | `machinfo` <br/> `machinfo \| grep -i memory` |
| 모델 넘버 확인 | `model` |
| 재부팅(reboot) | `shutdown -r 0` <br/> `shutdown -r now` <br/> `reboot` |  
| 중단(halt) | `shutdown -h 0` |  
 
### OS 정보
| 조작(Operation) | 명령어 |
|---|---|
| OS 정보 확인 | `osinfo` |
| 호스트네임 확인 | `hostname` <br/> `grep -i hostname /etc/rc.config.d/netconf` |
| 런레벨 확인 | `who -r` <br/> `grep initdefault /etc/inittab` |
| Release date 확인 | `uname -a` <br/> `uname -r` |
| 커널 구성 날짜(configuration date) 확인 | `kconfig -w` |
| 커널 파라미터 확인 | `kctune ← all parameter` <br/> `kctune -S ←  not default parameter` <br/> `kctune PARAMETER` |
| 커널 파라미터 세팅 | `kctune PARAMETER=VALUE` <br/> `kctune PARAMETER=Default ← change default` <br/> `kctune -h PARAMETER=VALUE` |
|  | `-h = changes will be held until next boot` |

### 네트워크
| 조작(Operation) | 명령어 |
|---|---|
| NIC 확인 | `nwmgr` |
| NIC 확인 | `lanscan` |
| 모든 NIC의 IP 나열 | `netstat -in ← no DNS` <br/> `netstat -i ← use DNS` |
| IP 확인 | `ifconfig NICNAME` <br/> `ifconfig lan0` |
| 라우팅 확인 | `netstat -rm` |
| NIC 속도 확인 | `lanadmin -x PPANUMBER` <br/> `lanadmin -x 0` |
| NIC Down 확인 | `ifconfig NICNAME down` |
| NIC Up 확인 | `ifconfig NICNAME up` |

### 유저
| 조작(Operation) | 명령어 |
|---|---|
| 시스템에 새로운 그룹 추가 | `groupadd -g GID GROUP` <br/> `groupadd -g 1000 dev` |
| 시스템에 새 사용자 로그인 추가 | `useradd -m USER` |
|  | `-m = Creates the home directory for the new login if it does not exist.` |
| 시스템으로부터 사용자 로그인 삭제 | `userdel USER` <br/> `userdel -r USER ← delete home directory` |
| 로그인 패스워드 변경 | `passwd` <br/> `passwd USER` |
| 패스워드 만료일 설정 | `passwd -x 90 USER ← 90 days` |
| 잠금된(Locked) 사용자 확인 | `userstat -u USER` |
| 사용자 잠금 해제 | `userdbset -d -u USER auth_failures` |
| 패스워드 규칙 설정 | `/usr/sbin/userdbset -u USER MIN_PASSWORD_LENGTH=8 PASSWORD_HISTORY_DEPTH=1 AUTH_MAXTRIES=10 PASSWORD_MIN_LOWER_CASE_CHARS=1 PASSWORD_MIN_UPPER_CASE_CHARS=1 PASSWORD_MIN_DIGIT_CHARS=1 PASSWORD_MIN_SPECIAL_CHARS=0` |
| 시스템에서 사용자 로그인 수정 | `usermod -u UID -g PRIMARY_GROUP -G GROUP USER` |

### 디스크
| 조작(Operation) | 명령어 |
|---|---|
| Utilization ratio | `bdf` |
| 디스크 사용량 요약 | `du -sk DIRECTORY` |
| 마운트 확인 | `mount` <br/> `mount -v` |
| 볼륨 마운트 | `mount -F vxfs /dev/vg01/lvol1 /u02` |
| 볼류 언마운트 | `umount /u02` |
| 마운트 프로세스 확인 | `fuser -u /u02` <br/> `pvdisplay /dev/disk/disk3_p2`|
| 볼륨 그룹 나열(list) | `vgdisplay` <br/> `vgdisplay -v` <br/> `vgdisplay -v vg01 \| grep "PV Name"`|
| 볼륨 그룹 활성화/비활성화 | `vgchange -a n /dev/vg01 ← deactivate` <br/> `vgchange -a y /dev/vg01 ← activate` |
| 논리 볼륨 나열(list) | `lvdisplay /dev/vg01/lvol02` |

### 스토리지
| 조작(Operation) | 명령어 |
|---|---|
| 스토리지 LUN 확인 | `ioscan -N -fnu -C disk` <br/> `ioscan -fnkC disk` <br/> `ioscan -m lun`|
| HP XP 스토리지 확인 | `xpinfo -i ← reduction` <br/> `xpinfo ← detail` <br/> `xpinfo -v ← check xpinfo version` |
| 스토리지 스캔 | `ioscan` <br/> `ioscan -f ← detail` |


### 소프트웨어
| 조작(Operation) | 명령어 |
|---|---|
| 소프트웨어 나열(list) | `swlist` |
| HP-UX 버전 | `swlist \| grep HPUX11i ` |
| HP-UX pach 버전 | `swlist -l bundle QPKBASE` <br/> `swlist -l bundle \| grep QPK` |
| 소프트웨어 설치 | `swinstall -s ./SOURCE.depot` <br/> `swinstall -s ./SOURCE.depot SOFTWARE` |
|  | `installed /usr/local` |
| 소프트웨어 제거 | `swremove ← GUI` <br/> `swremove SOFTWARE ← CLI` |

### Operation
| 조작(Operation) | 명령어 |
|---|---|
| ls | `ls -l` |
| sudo | `visudo` |
| 현재 성능 정보 | `top` <br/> `glance` |
| 메모리 정보 | `swapinfo` |
| 부하 평균 | `w` |
| 현재 로그인 정보 | `w` |
| 이전 로그인 정보 | `last` |

### 서비스
| 조작(Operation) | 명령어 |
|---|---|
| 서비스 중지 | `/sbin/init.d/SERVICE top` |
| 서비스 시작 | `/sbin/init.d/SERVICE start` |

