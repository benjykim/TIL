
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
| 하드웨어 확인(CPU, Memory, Firm, OS) | `machinfo` / `machinfo | grep -i memory` |
| 모델 넘버 확인 | `model` |
| 재부팅(reboot) | `shutdown -r 0` / `shutdown -r now` / `reboot` |  
| 중단(halt) | `shutdown -h 0` |  
 
### OS 정보
| 조작(Operation) | 명령어 |
|---|---|---|
| OS 정보 확인 | `osinfo` |
| 호스트네임 확인 | `hostname` / `grep -i hostname /etc/rc.config.d/netconf` |
| 런레벨 확인 | `who -r` / `grep initdefault /etc/inittab` |
| Release date 확인 | `uname -a` / `uname -r` |
| 커널 구성 날짜(configuration date) 확인 | `kconfig -w` |
| 커널 파라미터 확인 | `kctune ← all parameter` / `kctune -S ←  not default parameter` / `kctune PARAMETER` |
| 커널 파라미터 세팅 | `kctune PARAMETER=VALUE` / `kctune PARAMETER=Default ← change default` / `kctune -h PARAMETER=VALUE` |
|  | `-h = changes will be held until next boot` |

### 네트워크
| 조작(Operation) | 명령어 |
|---|---|
| NIC 확인 | `nwmgr` |
| NIC 확인 | `lanscan` |
| 모든 NIC의 IP 나열 | `netstat -in ← no DNS` / `netstat -i ← use DNS` |
| IP 확인 | `ifconfig NICNAME` / `ifconfig lan0` |
| 라우팅 확인 | `netstat -rm` |
| NIC 속도 확인 | `lanadmin -x PPANUMBER` / `lanadmin -x 0` |
| NIC Down 확인 | `ifconfig NICNAME down` |
| NIC Up 확인 | `ifconfig NICNAME up` |
