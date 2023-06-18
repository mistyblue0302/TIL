## 1. [필터(Filter)](https://github.com/dilmah0203/TIL/blob/main/JSP/%EC%84%9C%EB%B8%94%EB%A6%BF%20%ED%95%84%ED%84%B0.md)

디스패처 서블릿에 요청이 전달되기 전/후에 대해 부가작업을 처리할 수 있는 기능을 제공한다. 웹 컨테이너에 의해 관리가 된다.

## 2. 인터셉터(interceptor)

Spring이 제공하는 기술로, **디스패처 서블릿이 컨트롤러를 호출하기 전과 후에 부가적인 작업을 처리하는 기능을 제공한다.** 웹 컨테이너에서 동작하는 필터와 달리 인터셉터는 스프링 컨텍스트에서 동작한다. 인터셉터는 스프링 컨테이너 내에서 동작하므로 필터를 거쳐 프론트 컨트롤러인 디스패처 서블릿이 요청을 받은 이후에 동작한다. 

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Interceptor1.png)

### 인터셉터의 메소드

인터셉터를 추가하기 위해서는 `HandlerInterceptor` 인터페이스를 구현(implements)해야 하고 다음 3가지 메소드를 가지고 있다.

```java
public interface HandlerInterceptor { 
  
  default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) 
    throws Exception {} 
  
  default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, 
    @Nullable ModelAndView modelAndView) throws Exception {} 
  
  default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, 
    @Nullable Exception ex) throws Exception {} 
}
```

서블릿 필터의 경우 doFilter() 메소드 하나만 제공하지만, 인터셉터는 컨트롤러 호출 전/후, 요청 완료 이후와 같이 단계적으로 세분화되어 있다.

- **preHandle()** : **컨트롤러가 호출되기 전**에 실행되기 때문에 컨트롤러 이전에 전처리 작업에 사용한다. 리턴이 true 일경우 preHandle() 실행후 핸들러(컨트롤러)에 접근한다. false일경우더 이상 진행이 되지 않으며 핸들러 어댑터 또한 호출이 되지 않음.
  
- **postHandle()** : **컨트롤러 호출 이후**에 실행되기 때문에 컨트롤러 이후에 처리해야 하는 후처리 작업에 사용한다. 컨트롤러에서 예외가 발생하면 호출되지 않는다.
  
- **afterCompletion()** : **모든 작업이 완료**된 후에 실행되며 무조건 호출된다.(뷰 렌더링 이후) 예외와 무관하게 공통 처리를 하거나 예외 정보를 포함해서 호출할 수 있다.

### 인터셉터 설정

인터셉터는 `WebMvcConfigurer`를 구현한 클래스 내부에서 **addInterceptors()** 메소드를 오버라이딩하여 추가할 수 있다.

## 3. 필터 vs 인터셉터

인터셉터와 필터 모두 Controllor에 request가 도착하기 이전에 로직을 수행하거나 사용자에게 response가 전달되기 이전에 로직을 처리할 수 있도록 돕는다. 차이점은 **실행되는 시점**이 다르다.

- **필터**
  - 웹 컨테이너에 의해 관리(Web Context에 존재)
  - Spring 영역의 Dispatcher Servlet에 요청에 도착하기 이전과 response가 Dispatcher Servlet을 떠난 이후에 실행
  - 모든 요청에 로직이 필요한 경우에 사용한다. 인터셉터보다 앞단에서 동작하므로 웹 애플리케이션에 전반적으로 사용되는 기능을 구현하기에 적합

- **인터셉터**     
  - 스프링 컨테이너에 의해 관리(Spring Context에 존재)
  - 세부적으로 적용해야하는 인증 작업 등에 적합하기 때문에 특정 요청에만 전/후 로직이 필요할 경우 혹은 입/출력 데이터를 가공할 때 사용

### Request/Response 객체 조작 가능 여부

```java
public class FilterExample implements Filter {

    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
        chain.doFilter(request, response);       
    }
}
```

필터는 Request/Response를 다른 객체로 바꿀 수 있지만 인터셉터는 불가능하다. 필터가 다음 필터를 호출하기 위해서 필터 체이닝을 해주어야 한다. 이 때 Request/Response 객체를 넘겨줄 수 있다.

```java
public class InterceptorExample implements HandlerInterceptor {

    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        // Request/Response를 교체할 수 없고 boolean 값만 반환할 수 있다.
        return true;
    }
}
```

하지만 인터셉터는 필터와 처리과정이 다르다. true를 반환하면 다음 인터셉터가 실행되거나 컨트롤러로 요청이 전달되고, false가 반환되면 요청이 중단된다. 다른 Request/Response 객체를 넘겨줄 수 없다.

### 예외가 발생할 경우

필터는 웹 컨텍스트에서 동작하기 때문에 예외가 발생하면,
- web.xml에 error-page를 잘 등록해줘서 에러를 사용자에게 응답
- Filter내부에서 예외를 처리하기 위한 필터를 따로 둬서 try-catch문을 사용하여 예외 처리
- Filter 내부에서 try-catch 구문을 통해 예외 발생 시, HandlerExceptionResolver를 빈으로 주입받아 @ExceptionHandler에서 처리
    - 즉, 필터에서 발생하는 예외를 DispatcherServlet의 예외 처리기인 HandlerExceptionResolver에 보내서 처리하는 방식

인터셉터에서 예외가 발생하면, 인터셉터는 스프링이 관리하기 때문에 컨트롤러가 존재할 경우

- @ControllerAdvice, @ExceptionHandler로 예외 핸들링이 가능하다.

