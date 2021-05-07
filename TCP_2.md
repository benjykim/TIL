2. Server는 Client가 보낸 FIN에 응답하기 위해 K + 1 값을 Acknowledge Number로 설정하고 ACK 패킷을 전송한다.
3. 순서 2의 진행 후 Server의 애플리케이션이 내부적으로 종료 절차를 진행하면서 FIN을 전송한다. 순서 4의 ACK를 수신하지 못한 경우 수신할 때 까지 FIN을 재전송한다.
4. Client는 Server의 FIN에 대한 응답으로 L + 1 값을 Acknowledge Number에 설정하고 ACK 패킷을 전송한다.
 
 ```
[4Way-Handshacking Sample] 

55989 > 63111 [FIN, ACK] Seq=2564768963 Ack=2448526260 ...
63111 > 55989 [ACK] Seq=2448526260 Ack=2564768964 ...
63111 > 55989 [FIN, ACK] Seq=2448526260 Ack=2564768964
55989 > 63111 [ACK] Seq=2564768964 Ack=2448526261 ...
```

---

### 3Way-Handshacking과 4 Release의 상태 변화 정리

```mermaid
sequenceDiagram

Note left of Active Opener(Client): SYN_SENT(Active Open)
Active Opener(Client)->> Passive Opener(Server): SYN K
Note right of Passive Opener(Server): SYN_RCVD
Passive Opener(Server)->> Active Opener(Client): SYN L, ACK K + 1
Note left of Active Opener(Client): ESTABLISHED
Active Opener(Client)->> Passive Opener(Server): ACK L + 1
Note right of Passive Opener(Server): ESTABLISHED
Active Opener(Client) -->> Passive Opener(Server): [Data Transfer Proceeds in ESTABLISHED State]
Passive Opener(Server)-->> Active Opener(Client): [Data Transfer Proceeds in ESTABLISHED State]
Note left of Active Opener(Client): FIN_WAIT_1(Active Close)
Active Opener(Client)->> Passive Opener(Server): FIN M
Note right of Passive Opener(Server): CLOSE_WAIT(Passive Close)
Passive Opener(Server)->> Active Opener(Client): ACK M + 1
Note left of Active Opener(Client): FIN_WAIT_2
Note right of Passive Opener(Server): LAST_ACK
Passive Opener(Server)->> Active Opener(Client): FIN N
Note left of Active Opener(Client): TIME_WAIT(2MSL Timer)
Active Opener(Client)->> Passive Opener(Server): ACK N + 1
Note left of Active Opener(Client): CLOSED
Note right of Passive Opener(Server): CLOSED
```

---

* 출처: https://mr-zero.tistory.com/36?category=439699
