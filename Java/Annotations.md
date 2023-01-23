## Annotation

`@` 기호를 사용하는 문법 요소로 Java 5부터 생겼다. 어노테이션은 정보를 미리 약속된 형식으로 메소드나 클래스에 포함시켜 컴파일이나 런타임에 해석될 수 있다. 주석처럼 프로그래밍 언어에 영향을 미치지 않으면서도 유용한 정보를 제공한다.

어노테이션은 크게 두가지로 나뉜다.

- Built-in Annotation : 자바에서 기본적으로 제공하는 어노테이션
- Meta Annotation : 커스텀 어노테이션을 만들수 있게 제공된 어노테이션

### Built-in Annotation

|      종류      |           설명           |
| :------------: | :---------------------: |
|   @Deprecated  |  클래스나 메소드가 더 이상 사용되지 않음을 나타낸다. 해당 클래스나 메소드, 필드를 사용할 때 컴파일 경고가 발생한다.  |     
|   @Override    |  상위 클래스에 있는 메소드를 재정의한다는 것을 컴파일러에게 알린다.  |
|   @SuppressWarnings  |  컴파일러에서 경고를 알릴 때, 경고를 무시하는 어노테이션이다. |
|  @SafeVarargs  |  제너릭 같은 가변인자 매개변수를 사용할 때 경고를 무시한다. |
|   @FunctionalInterface   |  람다 함수등을 위한 인터페이스를 지정한다.  |

###  Meta Annotation

|      종류      |           설명           |
| :------------: | :---------------------: |
|   @Retention  |  얼마나 오래 어노테이션이 유지되는지 선언한다. 괄호 안에 적용 가능한 대상은 SOURCE, CLASS, RUNTIME이 있다.  |     
|   @Target    |  어노테이션 사용대상을 지정한다. 괄호에 적용 대상을 정해줄 수 있다. 대상에는 CONSTRUCTOR, FIELD, LOCAL_VARIABLE, PACKAGE, PARAMETER등이 있다.  |
|   @Documented  |  해당 어노테이션 정보가 자바 API문서에 포함된다는 것을 선언한다. |
|  @Inherited  |  모든 자식 클래스에서 부모 클래스의 어노테이션이 사용가능하다는 것을 선언한다. |

## 어노테이션의 사용 예

```java
public @interface MyAnnotation {
    String element1();
    int element() default 5; //default 키워드를 통해 디폴트 값 지정가능
}
```

`@interface`은 어노테이션 타입을 선언하는 키워드이다. 필드 리턴 타입은 Primitives, String, Class, enums, array로 제한된다.

### @Retention : 어노테이션이 유지되는 기간을 지정한다.
- RetentionPolicy.Source : 컴파일 전까지만 유효
- RetentionPolicy.CLASS : 컴파일러가 클래스를 참조할 때까지 유효(Default)
- RetentionPolicy.RUNTIME : 컴파일 이후에도 JVM에 의해 계속 참조 가능

### @Target : 어노테이션이 적용될 위치를 지정한다.

- ElementType.PACKAGE : 패키지선언
- ElementType.TYPE : 타입선언
- ElementType.CONSRTUCTOR : 생성자 선언
- ElementType.FIELD : 멤버변수 선언
- ElementType.LOCAL_VARIABLE : 지역 변수 선언
- ElementType.METHOD : 메서드 선언
- ElementType.PARAMETER : 전달인자 선언

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD})
public @interface MyAnnotation {
    String testDocument();
}
```

<br>

참고

https://docs.oracle.com/javase/tutorial/java/annotations/
