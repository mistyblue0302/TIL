## 웹 브라우저 요청 흐름

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Web%20Browser.png)

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/Web%20Browser3.png)

1. 웹 브라우저에 URL을 입력하면
2. 도메인 이름에 해당하는 IP 주소를 DNS에 요청한다.
3. DNS는 IP 주소를 응답으로 제공한다. 
4. 그리고 웹 브라우저는 HTTP 요청 메세지를 생성하여 서버에 전달한다.
5. 이 과정에서 IP 주소와 포트 정보를 SOCKET 라이브러리를 통해 TCP/IP 연결하여 데이터를 전달한다.
6. 서버는 HTTP 응답 메세지를 생성하고 웹 브라우저에 전달한다.
7. 웹 브라우저는 응답 받은 결과를 렌더링한다.

> URI란?
>
> URI는 특정 리소스를 식별하는 통합 자원 식별자(Uniform Resource Identifier)를 의미한다. URL은 흔히 웹 주소라고 하며 리소스가 어디있는지 알려주기 위한 규약이다.
> https://www.google.com/index.html은 웹 서버의 실제 리소스 위치를 나타내는 주소이므로 URI이면서 URL이다. 

### DNS(Domain Name System)

IP 주소는 기억하기 어렵고, 변경될 수 있는 특징을 가진다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/DNS.png)

DNS는 인터넷의 전화번호부와 같다. 위와 같이 DNS 서버에 도메인을 등록할 수 있다.
클라이언트는 `google.com` 도메인 이름을 통해서 접속한다. DNS 서버는 도메인에 해당하는 IP 주소를 응답으로 준다.
클라이언트는 해당 IP 주소로 접속하게된다. 이와 같은 방식으로 **도메인 이름을 IP 주소로 변환**하여 IP 주소의 문제점을 해결해준다.

