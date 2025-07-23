## 인터셉터 내부 동작원리

우선 디스패처 서블릿부터 살펴보면,

<img width="800" height="1000" alt="image" src="https://github.com/user-attachments/assets/9fa4ecd3-cf2f-4569-b91d-600218fb73d4" />

- Spring의 디스패처 서블릿에서 요청을 받으면, HandlerMapping을 통해 요청 URI에 매핑되는 핸들러(컨트롤러)를 찾는다.
- 이 때 내부적으로 핸들러(컨트롤러)객체와 해당 요청에 적용할 인터셉터들을 묶어 HandlerExecutionChain을 생성함.
- HandlerExecutionChain이 인터셉터의 preHandle()을 호출한다.
- 여기서 핸들러가 null이면 다음 요청으로 넘어가지 않기 때문에 핸들러에 대한 null 체크가 필요없음.
- HandlerExecutionChain 내부에서 인터셉터의 preHandle()이 호출될 때 파라미터로 받는 값인 HttpServletRequest, HttpServletResponse 객체는 이미 서블릿 컨테이너에서 생성된 객체이므로 null이 될 수 없다.


