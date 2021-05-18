# HOSTS.ALLOW 
리눅스에서는 `hosts.allow`와 `hosts.deny`에 설정을 해서 특정 IP의 접근을 허용 또는 차단할 수 있습니다.

### hosts.allow, hosts.deny
* `hosts.allow` : 기록되어 있는 IP와 프로세스 등의 접속을 허용함
* `hosts.deny` : 기록되어 있는 IP와 프로세스 등의 접속을 차단함

### hosts.allow, hosts.deny 예제
* `vi /etc/hosts.allow`
	```
	# 특정 프로세스 IP 접속 허용
	httpd 	 :ALL
	sshd	 :192.168.56.101
	sendmail :ALL
	mysql	 :ALL
	```
* `vi /etc/hosts.deny`
	```
	# 기본적으로 모든 IP와 프로세스 접속 차단
	ALL:ALL
	```
위와 같이 설정하면, `sshd`는 `192.168.56.101` IP만 해당 서비스를 접근하여 사용할 수 있다.
