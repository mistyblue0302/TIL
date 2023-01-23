## Annotation

`@` 기호를 사용하는 문법 요소로 Java 5부터 생겼다. 어노테이션은 정보를 미리 약속된 형식으로 메소드나 클래스에 포함시켜 해석될 수 있다. 

어노테이션은,

- 컴파일러에게 정보를 알려주거나
- 컴파일 시 또는 설치시 작업을 지정하거나
- 실행할 때 별도의 처리가 필요할 때 사용한다.
 
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

어노테이션을 선언 시에 사용한다.

|      종류      |           설명           |
| :------------: | :---------------------: |
|   @Retention  |  얼마나 오래 어노테이션이 유지되는지 선언한다. 괄호 안에 적용 가능한 대상은 SOURCE, CLASS, RUNTIME이 있다.  |     
|   @Target    |  어노테이션 사용대상을 지정한다. 괄호에 적용 대상을 정해줄 수 있다. 대상에는 CONSTRUCTOR, FIELD, LOCAL_VARIABLE, PACKAGE, PARAMETER등이 있다.  |
|   @Documented  |  해당 어노테이션 정보가 자바 API문서에 포함된다는 것을 선언한다. |
|  @Inherited  |  모든 자식 클래스에서 부모 클래스의 어노테이션이 사용가능하다는 것을 선언한다. |

**어노테이션의 사용 예**

어노테이션의 사용대상을 메소드로 지정하였다. 어노테이션 적용 시에 각 메소드의 이름에 해당하는 값을 괄호 안에 넣어주어야 한다. text()는 default 값을 지정해 주었기 때문에 값을 지정해주지 않아도 컴파일이 가능하지만, number()는 default 값이 지정되어 있지 않으므로 값을 정해주어야 한다. 

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)

public @interface AnnotationExample {

    public int number();

    public String text() default "hello";
}

@AnnotationExample(number = 2, text = "juseon")
public void example(){

}
```

<br>

참고

https://docs.oracle.com/javase/tutorial/java/annotations/
