---

### 스토리지
#### fdisk, vgscan, vgdisplay, lvscan, lvdisplay
* `fdisk -l * diskinfo /dev/rdisk/disk10` : HP-UX의 PV 디스크 정보
	* `fdisk` : 새로운 파티션 생성, 기존 파티션 삭제, 파티션 타입 결정 등의 작업 수행 가능
	* `fdisk -l` : 현재 모든 디스크의 파티션 설정 현황 볼 수 있다.
* `vgscan` : 디스크에 있는 볼륨 그룹(VG)을 검색하여 `/etc/lvmtab` 파일을 생성함. `fdisk`를 이용하여 파티션 속성을 LVM으로 지정한 후에 이 명령을 사용한다.
* `vgdisplay` : 볼륩 그룹의 속성과 정보를 보여주는 명령어 
	* `vgdisplay -v vgdata` : 좀 더 자세히 보여주는 옵션으로 VG 이외에 LV(Logical Volume)와 PV(Physical Volume)도 같이 보여준다.
* `lvscan` : 디스크에 있는 LV를 찾아준다.
* `lvdisplay -v` : LV의 정보를 보여준다.
* `mkfs.xfs /dev/vgdata/lvdata` : `/dev/vgdata/lvdata`파티션을 XFS 파일시스템으로 포맷한다.

#### /etc/fstab, mount, umount, fuser
* `/etc/fstab` : 파일 시스템 정보를 저장하고 있으며, 리눅스 부팅 시 마운트 정보를 저장하고 있는 파일이다. 
* `mount -a` : `fstab`에 있는 모든 파일 시스템을 마운트한다.
* `mount /dev/vgdata/lvdata /mnt` : 포맷한 볼륨을 `/mnt` 에 마운트
* `mount /data` : `/data` 폴더 마운트
* `umount /data` : `/data` 폴더 언마운트
* `fuser -cu /data` : `/data` 폴더 접근하고 있는 프로세스 조회
* `fuser -cku /data` : `/data` 폴더 접근하고 있는 프로세스 강제 Kill 
