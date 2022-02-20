# 💡 **import**

>  다른 패키지의 클래스를 불러와 사용하고자 할 때

<br>

**선언**

    import 패키지이름.클래스이름;
    import 패키지이름.*;

<br>

- **import static**

  - static 변수(클래스 변수)와 stiatic 메소드를 사용할 때 용이
  - import static을 사용하면 클래스 이름 생략 가능

<br>

```java
package c.javapackage.sub;

public class SubStatic{
    public final static String CLASS_NAME = "SubStatic";
    public static void subStaticMethod(){
        System.out.println("subStaticMethod is called.");
    }
}
```

<br>

```java
package c.javapackage;

import static c.javapackage.sub.SubStatic.subStaticMethod;
import static c.javapackage.sub.SubStatic.CLASS_NAME;
//import static c.javapackage.sub.SubStatic.*; 로도 사용가능

public class Static{
    public static void main(String[] args){
        subStaticMethod();
        System.out.println(CLASS_NAME);
    }
}
```

<br>

- **Package 클래스에 import한 동일한 이름의 stiatic 변수나 메소드가 있을 경우**

  - 자신의 클래스에 있는 static 변수나 메소드가 import static으로 가져온 것보다 우선

