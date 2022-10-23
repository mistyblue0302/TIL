## HTTP

> HTTP란? Hypertext Protocol의 약자로 서버-클라이언트 메세지 교환 프로토콜을 의미

![img](https://github.com/dilmah0203/TIL/blob/main/Image/HTTP%20communication.png)

클라이언트가 리소스를 HTTP를 통해 요청하면 TCP, IP 프로토콜을 걸쳐 서버쪽의 HTTP까지 요청이 도달한다. 이에 대한 응답을 다시 서버에서 HTTP로 보내면 TCP, IP 프로토콜을 통과해 클라이언트까지 응답이 도달하게 된다.

### HTTP 프로토콜 구성

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/HTTP%20Request.PNG)

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/HTTP%20Response.png)

### HTTP 프로토콜 특성

1. Client-Server 구조

각각의 Server와 Client는 독립되어 있다. Client는 Server에 요청(Request)를 보내고 응답이 올 때까지 대기하고, Server는 Client에서 받은 요청(Request)에 대한 결과를 만들어 응답을 하는 것이다.

2. Stateless(무상태)

HTTP는 과거 정보를 저장하지 않고 새로운 요청을 보낼 때마다 새로운 응답을 생성한다.

> 상태 유지가 필요할 때는? 세션과 쿠키를 이용한다.

3. Connectionless(비연결성)

클라이언트가 서버에 요청을 보내고 응답을 받게되면 바로 클라이언트와 서버의 연결을 끊어버린다. 따라서 서버는 최소한의 자원만을 사용하게 된다.

### HTTP 상태코드

- 1xx : 해당 요청에 대한 프로세스를 진행중

- 2xx : 요청이 성공적으로 처리됨
  - 200 : 서버가 요청을 성공적으로 처리함
  - 201 : 서버가 요청을 제대로 처리했고, 요청에 따른 새로운 리소스가 서버에 저장됨
  - 204 : 서버가 요청을 제대로 처리했지만, 요청에 따른 콘텐츠를 제공하지 않음

- 3xx : 클라이언트는 요청을 마치기 위해 추가 동작을 취해야 한다.

- 4xx : 클라이언트 오류로, 요청의 문법이 잘못되었거나 요청을 처리할 수 없음
  - 400 : 서버가 요청 구문을 인식하지 못함
  - 401 : 해당 요청을 들어주려면 인증이 필요함
  - 403 : 클라이언트가 리소스에 대한 필요 권한을 갖고 있지 않음(401은 인증 자체가 실패, 403은 인증은 됐지만 해당 사용자의 권한이 없을 때)
  - 404 : 해당 페이지를 찾을 수 없을 때

- 5xx : 서버 오류로, 요청을 명백하게 수행하지 못했음
  - 500 : 서버에 오류가 발생하여 요청을 수행할 수 없음
  - 501 : 서버에 요청을 처리할 수 있는 기능이 없음
  - 503 : 서버의 유지보수로 작동이 중단되거나 과부하가 걸린 상태

<br>

참고

[우아한Tech HTTP란?](https://www.youtube.com/watch?v=IjxkKQvn8Bc&list=PLgXGHBqgT2TvpJ_p9L_yZKPifgdBOzdVH&index=31)
