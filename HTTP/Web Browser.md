## 웹 브라우저 요청 흐름

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Web%20Browser.png)

도메인 이름에 해당하는 IP 주소를 DNS에 요청하고, DNS는 IP 주소를 응답으로 제공한다. 그리고 웹 브라우저는 HTTP 요청 메세지를 생성한다. 이 과정에서 찾은 IP주소와 포트 정보를
SOCKET 라이브러리를 통해 TCP/IP 연결하여 데이터를 전달한다.
서버는 HTTP 응답 메세지를 생성하고
웹 브라우저는 IP 주소를 이용해 웹 서버에 연결한 뒤 URL에 해당하는 웹 페이지를 요청하여 응답받게 된다.

![img3](https://github.com/dilmah0203/TIL/blob/main/Image/Web%20Browser3.png)
