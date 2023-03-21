## HTTP Header

헤더는 `field-name : field value`가 올 수 있다. field-name은 대소문자 구분이 없다. 
HTTP 전송에 필요한 모든 부가정보(메세지 바디의 내용, 크기, 인증, 서버 정보, 캐시 관리 정보)를 담고 있다. 

![img](https://github.com/dilmah0203/TIL/blob/main/Image/HTTP%20Header.png)

- 메세지 본문을 통해 표현 데이터 전달
- **표현**은 요청이나 응답에서 전달할 실제 데이터
- **표현 헤더는 표현 데이터**를 해석할 수 있는 정보 제공
  - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등

1. 표현
- Content-Type : 표현 데이터의 형식
- Content-Length : 표현 데이터의 길이
- Content-Language : 표현 데이터의 자연 언어
- Content-Encoding : 표현 데이터의 압축 방식

표현 헤더는 전송, 응답 둘다 사용된다.

2. 협상 : 클라이언트가 선호하는 표현 요청
- Accept : 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset : 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
- Accept-Language : 클라이언트가 선호하는 자연 언어

Quality Values(q)값을 사용하여 우선순위를 정할 수 있다. 0~1의 값으로 클수록 높은우선순위를 가진다. 협상 헤더는 요청시에만 사용한다.

3. 

