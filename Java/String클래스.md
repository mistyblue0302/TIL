## String 클래스

String 클래스는 문자열과 관련된 작업을 할 때 유용하게 사용할 수 있는 메소드들이 포함되어 있다.

**getBytes() : String에 대해 해당 문자열을 하나의 byte 배열로 변환**

같은 프로그램 내에서 문자열을 byte 배열로 저장하며, 
getBytes의 매개 변수를 통해 캐릭터 셋을 지정할 수 있다.

```java
public void convert() {
    try {
        String ex1 = "한글"; 
        byte[] array = ex1.getBytes(); 
        for (byte data : array) {
            System.out.print(data + " "); //-19 -107 -100 -22 -72 -128 
        }
        System.out.println();
        String ex2 = new String(array); //byte 배열로 Sring 객체를 생성
        System.out.println(ex2); //한글
    } catch (Exception e) {
        e.printStackTrace();
} 
```  

**length() : String의 내용을 비교/검색**

int로 문자열의 길이를 리턴하며 공백도 길이에 포함된다.

```java
public void checkLength(){
    String s = "Ju Seon";
    System.out.println(s.length()); //7
}
```

**isEmpty() : 문자열이 비어있을 경우 true를 리턴한다.**

```java
public void checkEmpty(){
    String s = "Ju Seon";
    System.out.println(s.isEmpty()); //false
}
```

**equals() : 문자열이 같을 경우 true를 리턴한다.**

```java
public void checkEquals(){
    String s1 = "Ju Seon";
    String s2 = "Ju Seon";
    if(s1==s2){
        System.out.println("same"); //same
    } else {
        System.out.println("different");
    }
    if(s1.equals(s2)){
        System.out.println("equals"); //equals
    }
}
```

**compareTo() : 매개 변수로 넘어가는 String 객체와 현재 객체가 같은지를 비교한다. 같으면 0, 순서 상으로 앞이면 -1, 뒤면 1을 리턴한다.**

```java
public void checkCompareTo(){
    String a = "a";
    String b = "b";
    String c = "c";
    System.out.println(b.compareTo(a)); //1
    System.out.println(b.compareTo(c)); //-1
    System.out.println(a.compareTo(c)); //-2
}
```

**startsWith() : 매개 변수로 넘겨준 값으로 시작하는지 확인한다.**

```java
public static void main(String[] args) {
    Example e = new Example();
        
    String address[] = new String[]{"서울시", "서울", "서울시청"};
    e.checkstartsWith(address);
}

public class Example {
    public void checkstartsWith(String[] address) {
        String start = "서울";
        int startCount = 0;
        for (String addresses : address) {
            if (addresses.startsWith(start)) {
                startCount++;
            }
        }
        System.out.println(startCount); //3
    }
}
```

**contains() : 매개 변수로 넘어온 값이 문자열에 존재하는지 확인한다.**

```java
public void checkContains(String[] address) {
    String text = "울";
    int containCount = 0;
    for (String addresses : address) {
        if (addresses.contains(text)) {
            containCount++;
        }
    }
    System.out.println(containCount); //3
}
```

**regionMatches() : 특정 영역이 매개 변수로 넘어온 문자열과 동일한지 확인 후 boolean 타입으로 리턴한다. 첫 번째 매개 변수가 true일 경우 대소문자 구분을 하지 않는다.**

```java
 public void checkMatch(){
    String text = "This is a Java";
    String check1 = "is";
    String check2 = "java";
    System.out.println(text.regionMatches(true, 2, check1, 0, 2)); //true
    System.out.println(text.regionMatches(false, 10, check2, 0, 4)); //false
}
```

**indexOf() : 앞에서부터 특정 문자열이나 char를 찾는다. 없으면 -1을 리턴한다. lastIndexOf()는 뒤에서부터 찾는다.**

```java
public void checkIndex() {
    String text = "My name is Juseon.";
    System.out.println(text.indexOf('s')); //9
    System.out.println(text.indexOf(" J")); //10
    System.out.println(text.indexOf('s', 11)); //13
}
```

**charAt() : 특정 위치의 char 값을 리턴한다.**

```java
public void checkCharAt(){
    String text = "Ju Seon";
    System.out.println(text.charAt(3)); //3
}
```

**substring() : 대상 문자열을 잘라 String으로 리턴한다. 시작 인덱스 부터 종료 인덱스 전까지 문자열의 부분 문자열을 반환한다.**

```java
public void checkSubString(){
    String text = "Ju Seon";
    String sub = text.substring(3);
    System.out.println(sub); //인덱스 3부터 자른다. Seon
}
```

**trim() : 대상 문자열의 맨 앞과 뒤에 있는 공백들을 제거한 문자열을 리턴한다.**

```java
public void checkTrim(){
    String text = "  My name is  ";
    String result = text.trim();
    System.out.println(result); //My name is
}
```

## String은 불변 객체다

String은 한 번 만들어지면 값이 변경될 수 없다. String 문자열을 더하게 되면 새로운 String 객체가 생성되고, 기존 객체는 버려진다. 그리고 나중에 GC의 대상이 된다.

이러한 String 클래스의 단점을 보완하기 위한 `StringBuffer`와 `StringBuilder`가 있다. `StringBuffer`는 Thread safe하며, `StringBuilder`는 Thread safe하지 않다. 속도는 StringBuilder 클래스가 빠르다. 두 클래스는 문자열을 더할 시에 새로운 객체를 생성하지 않는다. 

> 쓰레드 세이프(Thread Safe)란? 멀티 쓰레드 프로그래밍에서, 어떤 공유 자원에 여러 쓰레드가 동시에 접근해도 문제가 없는 것을 의미한다.

**그럼 언제 String을 사용할까?**

불변성을 가지기 때문에 변하지 않는 문자열을 자주 읽는 경우 사용한다. 문자열 추가와 수정 삭제 등의 연산이 빈번하게 발생하는 경우 String을 사용하게 되면 힙에 많은 객체가 생성되므로 StringBuffer / StringBuilder 클래스를 사용하는게 좋다.

<br>

참고 

https://docs.oracle.com/javase/8/docs/api/java/lang/String.html
