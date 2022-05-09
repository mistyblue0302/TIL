# 💡 **필터(Filter) vs 인터셉터(interceptor)**

1. 필터

디스패처 서블릿에 요청이 전달되기 전/후에 대해 부가작업을 처리할 수 있는 기능을 제공한다. 웹 컨테이너에 의해 관리가 된다.

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Filter2.png)


2. 인터셉터

Spring이 제공하는 기술로, 디스패처 서블릿이 컨트롤러는 호출하기 전과 후에 요청과 응답을 가공할 수 있는 기능을 제공한다. 웹 컨테이너에서 동작하는 필터와 달리 인터셉터는 스프링 컨텍스트에서 동작한다.

![img2](https://github.com/dilmah0203/TIL/blob/main/Image/Interceptor1.png)

인터셉터를 추가하기 위해서는 HandlerInterceptor 인터페이스를 구현(implements)해야 하며, 이는 다음의 3가지 메소드를 가지고 있다.

- preHandle 메소드
- postHandle 메소드
- afterCompletion 메소드

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

3. 필터와 인터셉터의 차이

필터는 원하는 Request/Response 객체를 넘겨줄 수 있지만 인터셉터는 넘겨줄 수 없다.

- 필터는 전역적으로 처리해야 하는 작업들을 처리할 수 있다. 인터셉터보다 앞단에서 동작하므로 웹 애플리케이션에 전반적으로 사용되는 기능을 구현하기에 적당하다.

- 인터셉터는 세부적으로 적용해야하는 인증 작업 등에 적합하다. 특정 그룹만 어떤 기능을 사용하지 못하는 경우에 처리하기 적합하다.
