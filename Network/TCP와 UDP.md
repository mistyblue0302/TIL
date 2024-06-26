## IP(Internet Protocol)

- OSI 7계층 중 네트워크 계층에서 사용하는 프로토콜이다.
- `192.168.1.35` 같은 형태를 IP 주소라고 하며 통신 상대가 되는 호스트를 식별하기 위한 식별 정보이다.
- 패킷 단위로 데이터를 전달하고 IP 패킷에는 출발지와 목적지 IP, 전송 데이터 등이 있다.

### IP의 특징

1. **비연결성** : 패킷을 받을 대상이 없거나, 서비스 불능 상태여도 패킷을 전송한다.
(예를 들어, 클라이언트는 대상 서버가 패킷을 받을 수 있는지 받을 수 없는 상태인지를 알 수가 없다.)

2. **비신뢰성** : 에러제어와 흐름제어를 하지 않는다. 중간에 패킷 소실 또는 패킷 전달 순서에 문제가 발생할 때 이러한 부분들을 조절해주는 작업을 하지 않는다.

IP 프로토콜만으로는 메세지가 제대로 전달되었음을 보장받을 수 없다. 이러한 문제를 TCP 프로토콜이 해결해준다.

## TCP(Transmission Control Protocol)

- OSI 7계층 중 전송 계층에서 사용하는 프로토콜이다.
- 데이터의 신뢰성과 순서 보장을 위해 사용한다.

### TCP의 특징

1. **연결형 서비스**
- 연결형 서비스로 **가상회선 방식**을 제공한다.
- 3-way handshaking 과정을 통해 연결을 설정한다.
- 4-way handshaking 을 통해 연결을 해제한다.

2. **흐름제어 및 혼잡제어**
   
- 흐름제어 : 수신측이 처리할 수 있는 속도에 맞추어 데이터를 전송, 윈도우 크기(Window Size) 값을 통해 수신량을 정할 수 있다.
- 혼잡제어 : 혼잡이 발생할 경우 송신측은 전송 속도를 조절

3. **신뢰성이 높은 전송**

- 정상적인 경우에는 ACK 값이 연속적으로 전송되어야 하지만 ACK 값이 중복으로 올 경우 패킷 이상을 감지하고 재전송을 요청
- 일정 시간동안 ACK 값을 수신하지 못할 경우 재전송을 요청

4. **순서 보장**

- 순서가 잘못된 경우 수신측이 송신측에 다시 전송 요청
- 시퀀스 번호((Sequence Number)를 사용

### TCP Header 정보

![img](https://github.com/dilmah0203/TIL/blob/main/Image/TCP%20Header.png)

| 필드 | 내용 | 크기(bit) | 
|-----|-----|-----|
|송수신자의 포트 번호|송수신 프로세스에 할당되는 포트 주소| 16 |
|시퀀스 번호(Sequence Number)|전송하는 데이터의 순서를 의미, 송신자가 최초로 데이터를 전송할 때는 이 번호를 랜덤한 수로 초기화 하며 이후 자신이 보낼 데이터의 1 bytes당 시퀀스 번호를 1씩 증가시키며 데이터의 순서를 표현	|32|
|응답 번호(ACK Number)|데이터를 받은 수신자가 예상하는 다음 시퀀스 번호를 의미, 수신 프로세스가 제대로 수신한 바이트의 수를 응답하기 위해 사용. 연결 설정과 연결 해제 때 발생하는 handshake 과정에서는 상대방이 보낸 시퀀스 번호 + 1로 자신의 응답 번호를 만들어낸다.|32|
|헤더 길이(Data Offset)|TCP 헤더의 크기|4|
|예약 필드(Reserved)|사용을 하지 않지만 나중을 위한 예약 필드이며 0으로 채워져야한다.|6|
|제어 비트(Flag Bit)|SYN, ACK, FIN 등의 제어 번호(아래 표 참고)|6|
|윈도우 크기(Window)|수신 윈도우의 버퍼 크기를 지정할 때 사용. 0이면 송신 프로세스의 전송 중지|16|
|체크섬(Checksum)|데이터를 송신하는 중에 발생할 수 있는 오류를 검출하기 위한 값|16|
|긴급 위치(Urgent Pointer)|긴급 데이터를 처리하기 위함, URG 플래그 비트가 지정된 경우에만 유효|16|

### 제어 비트(Flag Bit) 정보

|종류|내용|
|-|-|
|URG(Urgent)|긴급 위치의 필드가 유효한지 설정|
|ACK(Acknowledgement)|응답 유효 여부 설정. 최초의 SYN 패킷 이후 모든 패킷은 ACK 플래그 설정 필요. 데이터를 잘 받았으면 긍정 응답으로 ACK(=SYN+1) 전송|
|PSH(Push)|수신 애플리케이션에 버퍼링된 데이터를 상위 계층에 즉시 전달할 때|
|RST(Reset)|연결의 리셋이나 유효하지 않은 세그먼트에 대한 응답용|
|SYN(Synchronization)|연결 설정 요구. 양쪽이 보낸 최초의 패킷에만 이 플래그가 설정되어 있어야 한다.|
|FIN(Finish)|더 이상 전송할 데이터가 없을 때 연결 종료 의사 표시|

### ACK 제어비트

- ACK는 송신측에게 **수신측에서 긍정 응답**으로 보내지는 전송 제어용으로 사용된다.
- ACK Number를 사용하여 패킷이 도착했는지 확인하며, 송신한 패킷이 제대로 도착하지 않으면 **재송신을 요구**한다.

### TCP Connection (3-way Handshaking)

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/3-way%20handshake.png)

1.  open()을 실행한 클라이언트 A는 B 서버에 접속을 요청하는 `SYN(a)`을 보내고 `SYN_SENT` 상태로 대기한다.
2. B 서버는 `SYN_RECEIVED`로 상태를 바꾸고 클라이언트 A에게 `SYN(b)`와  응답 `ACK(a+1)`을 보낸다.
3. `SYN(b)`과 응답 `ACK(a+1)`을 받은 클라이언트는 `ESTABLISHED` 상태로 변경하고 서버에게 응답 `ACK(b+1)`를 보낸다.
4. 응답 `ACK(b+1)`를 받은 서버는 `ESTABLISHED` 상태로 변경한다.

### TCP Disconnection (4-way Handshaking)

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/4-way%20handshake.png)

1. close()를 실행한 클라이언트가 연결을 종료하겠다는 `FIN`을 보내고 `FIN_WAIT1` 상태로 대기한다.
2. 서버는 `CLOSE_WAIT`으로 바꾸고 응답 `ACK`를 전달한다. 동시에 해당 포트에 연결되어 있는 애플리케이션에게 close()를 요청한다.
3. `ACK`를 받은 클라이언트는 상태를 `FIN_WAIT2`로 변경한다.
4. close() 요청을 받은 서버 어플리케이션은 종료 프로세스를 진행하고 `FIN`을 클라이언트에 보내 `LAST_ACK` 상태로 바꾼다.
5. `FIN`을 받은 클라이언트는 `ACK`를 서버에 다시 전송하고 `TIME_WAIT`으로 상태를 바꾼다. `TIME_WAIT`에서 일정 시간이 지나면 `CLOSED`된다. `ACK`를 받은 서버도 포트를 `CLOSED`로 닫는다.

## UDP(User Datagram Protocol)

전송 계층에서 사용하는 프로토콜로서, 장치들 사이에 접속을 위하여 연결을 설정하여 신뢰성보다는 연속성이 중요한 비연결형 서비스다. 

### UDP의 특징

- 비연결형 서비스
- 데이터 제어 기능이 없다.
- 신뢰성이 낮은 전송
- TCP보다 속도가 빠르다. → 연결 설정이나 연결을 유지하지 않기 때문에

UDP는 비연결 서비스이기 때문에 연결을 설정하고 해제하는 과정이 없다. 또한 패킷에 순서를 부여하거나 흐름 제어, 혼잡 제어와 같은 기능도 처리하지 않기 때문에 TCP보다 속도가 빠르며 네트워크 부하가 적다는 장점이 있지만 신뢰성있는 데이터의 전송을 보장하지 못한다. 그렇기 때문에 신뢰성보다는 연속성이 중요한 실시간 서비스에 사용된다.

### UDP Header 정보

응용 계층으로부터 데이터를 받은 UDP도 UDP 헤더를 추가한 후에 이를 IP(네트워크 계층)로 보낸다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/UDP%20Header.png)

|필드|내용|크기(bit)|
|--|---|--------|
|송수신자의 포트 번호|데이터를 보내는 받는 애플리케이션의 포트 번호|16|
|데이터의 길이(length)|UDP 헤더와 데이터의 총 길이|16|
|체크섬(Checksum)|데이터 오류 검사에 사용|16|

TCP 헤더와 달리 UDP 헤더에는 포함된 정보가 적다. 이는 UDP는 수신자가 데이터를 받는지 여부에 대해 관심이 없기 때문이다.

### TCP vs UDP

|TCP(Transfer Control Protocol)|UDP(User Datagram Protocol)|
|-|-|
|연결이 성공해야 통신 가능(연결형 프로토콜)|비연결형 프로토콜(연결 없이 통신이 가능)|
|전송 순서 보장|전송 순서를 보장하지 않는다.|
|느리다.||빠르다.|
|가상 회선 방식|데이터그램 방식|
|데이터의 경계를 구분하지 않음(Byte-Stream Service)|데이터의 경계를 구분함(Datagram Service)|
|신뢰성 있는 데이터 전송(3-way Handshaking, 4-way Handshaking을 통해)|비신뢰성 있는 데이터 전송(데이터의 재전송 없음)|
|일 대 일(Unicast) 통신|일 대 일, 일 대 다(Broadcast), 다 대 다(Multicast) 통신|

### 가상회선 패킷 교환 방식

`TCP`는 패킷 사이의 순서를 보장한다. 연결 지향 프로토콜을 사용해서 신뢰성을 구축해서 수신 여부를 확인하며 `가상 회선 패킷 교환 방식`을 사용한다.

![img4](https://github.com/dilmah0203/TIL/blob/main/Image/Virtual%20Circuit_Packet.png)

데이터를 전송하기 전에 논리적 연결이 설정되는데, 이를 가성회선이라고 한다.(연결 지향형) 각 패킷에는 가상회선 식별 번호(VCI)가 포함되고, 모든 패킷을 전송하면 가상회선이 해제되고 패킷들은 전송된 순서대로 도착한다.

### 데이터그램 패킷 교환 방식

`UDP`는 패킷 사이의 순서를 보장하지 않고 수신 여부를 확인하지 않으며 단순히 데이터만 주는 `데이터그램 패킷 교환 방식`을 사용한다.

![img5](https://github.com/dilmah0203/TIL/blob/main/Image/Datagram_Packet.png)

데이터를 전송하기 전에 논리적 연결이 설정되지 않으며 패킷이 독립적으로 전송된다. 이를 데이터그램이라 한다. 패킷을 수신한 라우터는 최적의 경로를 선택하여 패킷을 전송하는데 하나의 메시지에서 분할된 여러 패킷은 서로 다른 경로로 전송될 수 있다.(비연결 지향형) 송신 측에서 전송한 순서와 수신 측에 도착한 순서가 다를 수 있다.

<br>

참고

[What is Transmission Control Protocol (TCP)?](https://www.geeksforgeeks.org/what-is-transmission-control-protocol-tcp/)

[TCP와 UDP의 특징과 차이](https://mangkyu.tistory.com/15)

[TCP의 3 way handshake와 4 way handshake](https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md#tcp%EC%9D%98-3-way-handshake%EC%99%80-4-way-handshake)
