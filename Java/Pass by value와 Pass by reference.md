# 💡 **Pass by value와 Pass by reference**

<br>

- "Pass by value"

  - 값만 전달
  - 기본 자료형을 매개변수로 넘겨줄 때

<br>

```java
public class Main {
  public void swap(int x, int y){
        int temp = x;
        x = y;
        y = temp;
  }

  public static void main(String[] args){
        Main m = new Main();
        
        int a = 3;
        int b = 5;

        System.out.println("a ="+a+","+"b ="+b);
        m.swap(a,b);
        System.out.println("a ="+a+","+"b ="+b);
  }
}
```

```java
> a =3,b =5
> a =3,b =5
```

<br>

- "Pass by reference"

  - 참조(주소)를 전달
  - 메소드에서 파라미터 수정 시 원본 데이터에도 영향이 있다

<br>

```java
public class MemberDTO {
    public String name;
    public String phone;
    public String email;
    public MemberDTO(String name){
        this.name=name;
    }
}
```

```java
public class Main {
    public void callPassByReference(){
        MemberDTO member = new MemberDTO("Lee");
        System.out.println("before : "+member.name);
        passByReference(member);
        System.out.println("after : "+member.name);

    }
    public void passByReference(MemberDTO member){
        member.name = "Kim";
        System.out.println("In : "+member.name);
    }
    public static void main(String[] args){
        Main m = new Main();
        m.callPassByReference();
    }
}
```

```java
> before : Lee
> In : Kim
> after : Kim
```

