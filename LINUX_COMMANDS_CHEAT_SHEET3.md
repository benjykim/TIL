### 유저 관리(Administration)

#### 유저
| 명령어 | 설명 | 예시 | 
|---|---|---|
| useradd | create a new user or update default new user information |  |
| adduser | add a user to the system |  |
| whoami | print effective userid |  |
| w | Show who is logged on and what they are doing |  |
| who | show who is logged on |  |
| userdel | delete a user account and related files |  |
| vipw | edit the password, group, shadow-password or shadow-group file |  |
| passwd | change user password |  |
| chpasswd | update passwords in batch mode |  |
| chage | change user password expiry information |  |
| usermod | modify a user account |  |
| gpasswd |  |  |
| chsh | change login shell |  |
| getent | get entries from Name Service Switch libraries |  |
| pam_tally2 | The login counter (tallying) module |  |

#### 그룹
| 명령어 | 설명 | 예시 | 
|---|---|---|
| groups | print the groups a user is in |  |
| groupadd | create a new group |  |
| addgroup | add group to the system |  |
| groupdel | delete a group |  |
| groupmod | change USER's GID |  |
| chgrp | change the Group of the file |  |
| vigr | edit the password, group, shadow-password or shadow-group file |  |

#### 파일 권한
| 명령어 | 설명 | 예시 | 
|---|---|---|
| chmod | change file mode bits |  |
| chown | change file owner and group |  |

#### ETC
| 명령어 | 설명 | 예시 | 
|---|---|---|
| finger | user information lookup program |  |
| su | change user ID or become superuser |  |
| sudo | execute a command as another user |  |
| id | print real and effective user and group IDs |  |
| last | show listing of last logged in users |  |
| lastlog | reports the most recent login of all users or of a given user |  |
| umask | set file mode creation mask |  |

### 네트워크

#### 네트워크
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ip | show / manipulate routing, devices, policy routing and tunnels |  |
| ss | another utility to investigate sockets |  |
| ifconfig | configure a network interface |  |
| ifdown | take a network interface down |  |
| ifup | bring a network interface up |  |
| route | show / manipulate the IP routing table |  |
| ethtool | Display or change ethernet card settings |  |
| mii-tool | view, manipulate media-independent interface status |  |
| arp | manipulate the system ARP cache |  |
| nmcli | line tool for controlling NetworkManager |  |
| tcpdump | dump traffic on a network |  |

#### 네트워크 연결 확인
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ping | send ICMP ECHO_REQUEST to network hosts |  |
| traceroute | print the route packets trace to network host |  |
| tracepath | traces path to a network host discovering MTU along this path |  |
| mtr | a network diagnostic tool |  |
| nmap | Network exploration tool and security / port scanner |  |
| nc <br/> netcat | Concatenate and redirect sockets |  |
| httping | measure the latency and throughput of a webserver |  |

#### DNS
| 명령어 | 설명 | 예시 | 
|---|---|---|
| dig | DNS lookup utility |  |
| nslookup | query Internet name servers interactively |  |
| host | DNS lookup utility |  |
| whois | client for the whois service |  |
| nscd | name service cache daemon |  |

#### Connection
| 명령어 | 설명 | 예시 | 
|---|---|---|
| telnet | user interface to the TELNET protocol |  |
| ssh | OpenSSH SSH client (remote login program) |  |
| scp | secure copy (remote file copy program) |  |
| rsync | a fast, versatile, remote (and local) file-copying tool |  |
| ssh-keygen | authentication key generation, management and conversion |  |
| ssh-copy-id | use locally available keys to authorise logins on a remote machine |  |

#### HTTP
| 명령어 | 설명 | 예시 | 
|---|---|---|
| curl | transfer a URL |  |
| wget | The non-interactive network downloader | |


#### SNMP

| 명령어 | 설명 | 예시 | 
|---|---|---|
| snmpwalk | retrieve a subtree of management values using SNMP GETNEXT requests |  |
| snmpget | communicates with a network entity using SNMP GET requests |  |
|snmptranslate  | translate MIB OID names between numeric and textual forms |  |
| snmpnetstat | display networking status and configuration information from a network entity via SNMP |  |
