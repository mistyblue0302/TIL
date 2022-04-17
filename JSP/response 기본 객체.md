# 💡 **response 기본 객체**

response 기본 객체는 request 기본 객체와 반대의 기능을 수행한다. 
request 기본 객체가 웹 브라우저가 전송한 요청 정보를 담고 있다면 response 기본 객체는 웹 브라우저에 보내는 응답 정보를 담는다.

- 헤더 정보 입력

  add로 시작하는 메소드는 기존의 헤더에 새로운 값을 추가할 때 사용하고, set로 시작하는 메소드는 헤더의 값을 새로 지정할 때 사용한다.

  - addDateHeader(String name, long date) : name 헤더에 date를 추가한다.
  
  - addHeader(String name, String value) : name 헤더에 value를 값으로 추가한다.

  - addIntHeader(String name, int value) : name 헤더에 정수 값 value를 추가한다.

  - setDateHeader(String name, long date) : name 헤더의 값을 date로 지정한다.

  - setHeader(String name, String value) : name 헤더의 값을 value로 지정한다.

  - containsHeader(String name) : 이름이 name인 헤더를 포함하고 있을 경우 true를, 아닐 경우 false를 리턴한다.



- 리다이렉트

  웹 서버 측에서 웹 브라우저에게 어떤 페이지로 이동하라고 지정하는 것이 리다이렉트 기능이다. response 기본 객체는 다음 메소드를 사용하여 리다이렉트를 지시한다.

  - response.sendRedirect(String location)