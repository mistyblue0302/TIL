## 1. [필터(Filter)](https://github.com/dilmah0203/TIL/blob/main/JSP/%ED%95%84%ED%84%B0.md)

디스패처 서블릿에 요청이 전달되기 전/후에 대해 부가작업을 처리할 수 있는 기능을 제공한다. 웹 컨테이너에 의해 관리가 된다.

## 2. 인터셉터(interceptor)

Spring이 제공하는 기술로, **디스패처 서블릿이 컨트롤러를 호출하기 전과 후에 부가적인 작업을 처리하는 기능을 제공한다.** 웹 컨테이너에서 동작하는 필터와 달리 인터셉터는 스프링 컨텍스트에서 동작한다. 인터셉터는 스프링 컨테이너 내에서 동작하므로 필터를 거쳐 프론트 컨트롤러인 디스패처 서블릿이 요청을 받은 이후에 동작한다. (실제로는 Interceptor가 Controller로 요청을 위임하지는 않는다)

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Interceptor1.png)

### 인터셉터의 메소드

인터셉터를 추가하기 위해서는 HandlerInterceptor 인터페이스를 구현(implements)해야 하며, 다음 3가지 메소드를 가지고 있다.

```java
public interface HandlerInterceptor { 
  
  default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) 
    throws Exception { 
    
    return true; 
  } 
  
  default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, 
    @Nullable ModelAndView modelAndView) throws Exception {
  } 
  
  default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, 
    @Nullable Exception ex) throws Exception {
  } 
}
```

- **preHandle()**
  - 컨트롤러가 호출되기 전에 실행되기 때문에 컨트롤러 이전에 전처리 작업에 사용한다.
  
- **postHandle()**
  - 컨트롤러 호출 이후에 실행되기 때문에 컨트롤러 이후에 처리해야 하는 후처리 작업에 사용
  
- **afterCompletion()**
  - 모든 작업이 완료된 후에 실행된다.

### 인터셉터 설정

인터셉터는 WebMvcConfigurer를 구현한 클래스 내부에서 addInterceptors() 메소드를 오버라이딩하여 추가할 수 있다.

## 3. 필터 vs 인터셉터

- **필터**
  - 웹 컨테이너에 의해 관리
  - 전역적으로 처리해야 하는 작업들을 처리할 수 있다. 인터셉터보다 앞단에서 동작하므로 웹 애플리케이션에 전반적으로 사용되는 기능을 구현하기에 적합

- **인터셉터**     
  - 스프링 컨테이너에 의해 관리 
  - 세부적으로 적용해야하는 인증 작업 등에 적합하기 때문에 특정 그룹만 어떤 기능을 사용하지 못하는 경우에 처리하기 적합

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

