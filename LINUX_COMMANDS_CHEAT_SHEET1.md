# LINUX (RHEL/CENTOS) 명령어 정리
### 목차

* 시스템 명령어
	* 시스템 명령어
	* 프로세스 관리(Management)
	* 스케줄
	* ETC
* 파일 및 디렉토리 명령어
	* 파일 명령어
	* 디렉터리 명령어
	* 파일 내용에 접근하는 명령어
	* 검색
* 유저 관리(Administration)
	* 유저
	* 그룹
	* 파일 권한
	* ETC
* 네트워크
	* 네트워크
	* 네트워크 연결 확인
	* DNS
	* 연결(Connection)
	* HTTP
	* SNMP
* 하드웨어
	* 하드웨어
	* 모듈
* 디스크 유틸(Utilities)
	* HDD
	* 파티션
	* Swap
	* 파일 시스템
	* 데이터
	* 마운트
* 성능
	* 성능

---

### 시스템 명령어
#### 시스템 명령어
| 명령어 | 설명 | 예시 |
|---|---|---|
| shutdown | bring the system down | shutdown -h now ← -h = Halt or poweroff after shutdown <br/> shutdown -r now ← -r = Reboot after shutdown <br/> shutdown -r -F now ← -F = Force fsck after reboot.  |
| halt | stop the system | halt |
| reboot | reboot the system. | reboot |
| init |  | init 1 #change to single usermode |
| uptime | Tell how long the system has been running. | uptime |
| runlevel | find the previous and current system runlevel. | runlevel |
| printenv | print all or part of environment | printenv |
| env | run a program in a modified environment | env |
| hostname | show or set the system's host name | hostname ← show the system's host name <br/> I recommend  `uname -n`  for check hostname. <br/> hostname NEWHOSTNAME ← set the system's host name |
| uname | print system information | uname -a ← print all information (=uname --all) <br/> uname -n ← show the system's host name (=uname --nodename) |
| locale | Get locale-specific information. | locale <br/> locale -a |grep -i ja <- -a : --all-locales |

#### 프로세스 관리(Management)
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ps | report a snapshot of the current processes | ps aux \| grep httpd ← Check httpd process <br/> ps aux \| grep XXX \| awk '{print $2}' \| xargs kill -9 <br/> ps auxwf <br/> ps auxwf \| grep XXX |
| pgrep | look up processes based on name and other attributes | pgrep -f 'bash' <br/> pgrep -lf 'bash' ← output with process name <br/> pgrep -f 'bash \| xargs kill |
| pstree | display a tree of processes | pstree -a |
| pidof | find the process ID of a running program | pidof httpd <br/> /bin/kill $(/sbin/pidof qmail-popup) |
| kill | send a signal to a process | kill -9 PID ← -9 or -KILL = force-quit |
| pkill | signal processes based on name and other attributes | pkill -f 'bash' <br/> pkill -u user1 <br/> pkill java <br/> pkill -f jar |
| killall | kill processes by name | killall vi <br/> killall -i vi ← -i = interactively <br/> killall -HUP kterm |
| killproc |  |  |
| lsof | list open files | lsof -i <br/> lsof -i -P ← no port names <br/> lsof -i :80,443 ← Which process is using Port 80,443 |
| Ctrl + C | Stop running process |  |
| Ctrl + Z | Suspend running process | Move Running Process to Background <br/> 1. ctrl + z <br/> 2. jobs <br/> 3. bg <br/> 4. disown %JOBID |
| jobs | The first form lists the active jobs. |  |
| fg |Resume jobspec in the foreground  |  |
| bg | Resume each suspended job jobspec in the background | jobs -l ← List job |
| nohup | run a command immune to hangups, with output to a non-tty | nohup command.sh & |
| disown |  | disown %JOBID |
| nice | run a program with modified scheduling priority | nice -n 19 test.sh <br/> nice -n 19 ionice -c 3 CMD <br/> nice -n 19 ionice -c 2 -n 7 COMMAND |
| renice | alter priority of running processes | renice 19 -p PID <br/> you can check the nice with "top" or "ps alx" |
| ionice | sets or gets process io scheduling class and priority | ionice -p PID ← check <br/> ionice -c 3 -p PID <br/> nice -n 19 ionice -c 2 -n 7 COMMAND |

#### 스케줄
| 명령어 | 설명 | 예시 |
|---|---|---|
| crontab | maintain crontab files | crontab -l ← -l = list user's crontab <br/> crontab -u USER -l <br/> crontab -e ← -e = edit user's crontab <br/> crontab -u USER -e  |
| at | queue jobs for later execution | echo "/sbin/shutdown -h now" \| at 21:00 02/30/2009 <br/> at -t 200902302100  |
| atq | lists the user's pending jobs | atq |
| atrm | delete jobs for later execution | atrm JOBID |
| watch | execute a program periodically, showing output fullscreen | watch ntpq -p ← By default, the program is run every 2 seconds <br/> watch -n 1 ntpq -p ← 1 seconds interval <br/> -d = highlight the differences between successive updates|
