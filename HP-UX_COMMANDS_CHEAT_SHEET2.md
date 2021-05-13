### 유저
| 조작(Operation) | 명령어 |
|---|---|
| 시스템에 새로운 그룹 추가 | `groupadd -g GID GROUP` / `groupadd -g 1000 dev` |
| 시스템에 새 사용자 로그인 추가 | `useradd -m USER` |
|  | `-m = Creates the home directory for the new login if it does not exist.` |
| 시스템으로부터 사용자 로그인 삭제 | `userdel USER` / `userdel -r USER ← delete home directory` |
| 로그인 패스워드 변경 | `passwd` / `passwd USER` |
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
| 마운트 확인 | `mount` / `mount -v` |
| 볼륨 마운트 | `mount -F vxfs /dev/vg01/lvol1 /u02` |
| 볼류 언마운트 | `umount /u02` |
| 마운트 프로세스 확인 | `fuser -u /u02` / `pvdisplay /dev/disk/disk3_p2`|
| 볼륨 그룹 나열(list) | `vgdisplay` / `vgdisplay -v`  / `vgdisplay -v vg01 | grep "PV Name"`|
| 볼륨 그룹 활성화/비활성화 | `vgchange -a n /dev/vg01 ← deactivate` / `vgchange -a y /dev/vg01 ← activate` |
| 논리 볼륨 나열(list) | `lvdisplay /dev/vg01/lvol02` |

### 스토리지
| 조작(Operation) | 명령어 |
|---|---|
| 스토리지 LUN 확인 | `ioscan -N -fnu -C disk` / `ioscan -fnkC disk` / `ioscan -m lun`|
| HP XP 스토리지 확인 | `xpinfo -i ← reduction` / `xpinfo ← detail` / `xpinfo -v ← check xpinfo version` |
| 스토리지 스캔 | `ioscan`  / `ioscan -f ← detail` |
