# 💡 **String 클래스**

String 클래스는 문자열과 관련된 작업을 할 때 유용하게 사용할 수 있는 메소드들이 포함되어 있다.


다음 결과는 서로 같다.

```java
String str = "abc";

char data[] = {'a', 'b', 'c'};
String str = new String(data);
```

<br>

**String 문자열을 byte로 변환**

**getBytes()**

같은 프로그램 내에서 문자열을 byte 배열로 저장하며, 
getBytes의 매개변수를 통해 캐릭터 셋을 지정할 수 있음


```java
public class Example {

    public static void main(String[] args) {
        Example e = new Example();
        e.convert();
    }

    public void convert() {
        try {
            String ex1 = "이주선"; //String 객체 생성
            byte[] array = ex1.getBytes(); //문자열을 배열로 저장
            for (byte data : array) {
                System.out.print(data + " ");
            }
            System.out.println();
            String ex2 = new String(array); //byte 배열로 Sring 객체를 생성
            System.out.println(ex2);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```  
<br>

**String의 내용을 비교/검색**

**length()**

int로 문자열의 길이를 리턴하며 공백도 길이에 포함된다

```java
public void checkLength(){
        String s = "Ju Seon";
        System.out.println(s.length()); //7
    }
```
<br>


**isEmpty()**

문자열이 비어있을 경우 true를 리턴한다

```java
public void checkEmpty(){
        String s = "Ju Seon";
        System.out.println(s.isEmpty()); //false
    }
```
<br>

**equals()**

문자열이 같을 경우 true를 리턴한다
메소드 이름에 IgnoreCase가 붙을 경우, 대소문자 구분을 하지 않는다.

<br>

**compareTo()**

보통 정렬 시에 사용하며 매개변수의 String 객체가 알파벳 순으로 앞에 있으면 양수를, 뒤에 있으면 음수를 리턴한다

```java
public void checkCompareTo(){
        String a = "a";
        String b = "b";
        String c = "c";
        System.out.println(b.compareTo(a)); // 1
        System.out.println(b.compareTo(c)); // -1
        System.out.println(a.compareTo(c)); // -2
    }
```
<br>

