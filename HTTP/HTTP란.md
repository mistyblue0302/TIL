## **HTTP란**

HTTP는 HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜로, 기본적으로 요청/응답 (request/response) 구조로 되어있다.

### **HTTP 통신 방식**

클라이언트가 HTTP request를 서버에 보내면 서버는 HTTP response를 보내는 구조이며 클라이언트와 서버의 모든 통신이 요청과 응답으로 이루어 진다.

### **HTTP Request 구조**

HTTP request 메세지는 크게 3부분으로 구성된다:

- start line
- headers
- body

1. **start line**

>  GET /search HTTP/1.1

- HTTP Method : request가 의도한 action을 정의하는 부분으로 GET, POST, PUT, DELETE 등이 있다.
- Request target : request가 전송되는 URI
- HTTP Version : 사용되는 HTTP 버전

2. **headers**

   해당 request에 대한 추가 정보(addtional information)를 담고 있는 부분
  
- Host : 요청이 전송되는 target의 host url
- Accept : 요청이 받을 수 있는 응답(response) 타입.
- Connection : 요청이 끝난후에 클라이언트와 서버가 계속해서 네트워크 컨넥션을 유지 할것인지 아니면 끊을것인지에 대한 부분
- Content-Type : 요청이 보내는 메세지 body의 타입. 예를 들어, JSON을 보내면 application/json
- Content-Length: 메세지 body의 길이

3. **body**

   reqeust의 실제 메세지/내용으로, body가 없는 request도 있다.

### **HTTP Response 구조**

- status line
- headers
- body

1. **status line**

> HTTP/1.1 404 Not Found

- HTTP Version
- status code: 응답 상태를 나타내는 코드로 숫자로 되어있다.
- status text: 응답 상태를 간략하게 설명해주는 부분

2. **headers**

   response의 headers와 동일한데 response에서만 사용되는 header 값들이 있다.
   예를 들어, User-Agent 대신에 Server 헤더가 사용된다.
   
3. **body**

    reqeust의 body와 일반적으로 동일
