# 💡 **Pass by value와 Pass by reference**

<br>

- "Pass by value"

  - 값만 전달
  - 기본 자료형을 매개변수로 넘겨줄 때

<br>

```java
public void callPassByValue(){
    int a = 2;
    int b = 3;
    System.out.println("before PassByValue"+" a="+a+", b="+b);

    passByValue(a,b);
    System.out.println("after PassByValue"+" a="+a+", b="+b);

}

public void passByValue(int a, int b){
    a = 10;
    b = 20;
    System.out.println("In PassByValue"+" a="+a+", b="+b);

}
```

```java
> before PassByValue a=2, b=3
> In PassByValue a=10, b=20
> after PassByValue a=2, b=3
```

<br>

- "Pass by reference"

  - 참조(주소)를 전달
  - 메소드에서 파라미터 수정 시 원본 데이터에도 영향이 있다

<br>

```java
public void callPassByReference(){
    String a = "a";
    String b = "b";
    System.out.println("before PassByReference:"+" a="+a+", b="+b);

    passByReference(a,b);
    System.out.println("after PassByReference:"+" a="+a+", b="+b);

}

public void passByReference(String a, String b){
    a = "c";
    b = "d";
    System.out.println("In PassByReference"+" a="+a+", b="+b);

}
```

```java
> before PassByReference: a=a, b=b
> In PassByReference a=c, b=d
> after PassByReference: a=a, b=b
```

