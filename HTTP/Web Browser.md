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
8. 
