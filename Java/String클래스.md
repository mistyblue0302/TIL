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
getBytes의 매개 변수를 통해 캐릭터 셋을 지정할 수 있다.


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

int로 문자열의 길이를 리턴하며 공백도 길이에 포함된다.

```java
public void checkLength(){
        String s = "Ju Seon";
        System.out.println(s.length()); //7
}
```
<br>


**isEmpty()**

문자열이 비어있을 경우 true를 리턴한다.

```java
public void checkEmpty(){
        String s = "Ju Seon";
        System.out.println(s.isEmpty()); //false
}
```
<br>

**equals()**

문자열이 같을 경우 true를 리턴한다.
메소드 이름에 IgnoreCase가 붙을 경우, 대소문자 구분을 하지 않는다.

```java
public void checkEquals(){
        String s1 = "Ju Seon";
        String s2 = "Ju Seon";
        if(s1==s2){
            System.out.println("same"); //same
        }else {
            System.out.println("different");
        }
        if(s1.equals(s2)){
            System.out.println("equals"); //equals
        }
}

public void checkEquals(){
        String s1 = "Ju Seon";
        String s2 = new String("Ju Seon");
        if(s1==s2){
            System.out.println("same");
        }else {
            System.out.println("different"); //different
        }
        if(s1.equals(s2)){
            System.out.println("equals"); //equals
        }
}
```

<br>

**compareTo()**

보통 정렬 시에 사용하며 매개 변수의 String 객체가 알파벳 순으로 앞에 있으면 양수를, 뒤에 있으면 음수를 리턴한다. 차이나는 만큼 숫자값은 커진다.

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
<br>

**startsWith()**

매개 변수로 넘겨준 값으로 시작하는지 확인한다.

```java
public static void main(String[] args) {
        Example e = new Example();
        
        String address[] = new String[]{"서울시", "서울", "서울시청"};
        e.checkstartsWith(address);
}

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
```
<br>

**contains()**

매개 변수로 넘어온 값이 문자열에 존재하는지 확인한다.

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
<br>

**regionMatches()**

특정 영역이 매개 변수로 넘어온 문자열과 동일한지 확인 후 boolean 타입으로 리턴한다. 첫 번째 매개 변수가 true일 경우 대소문자 구분을 하지 않는다.

```java
 public void checkMatch(){
        String text = "This is a Java";
        String check1 = "is";
        String check2 = "java";
        System.out.println(text.regionMatches(true, 2, check1, 0, 2)); //true
        System.out.println(text.regionMatches(false, 10, check2, 0, 4)); //false
}
```

<br>

**String에서 위치를 찾아내는 메소드**

**indexOf()**

앞에서부터 특정 문자열이나 char를 찾는다. 없으면 -1을 리턴한다. lastIndexOf()는 뒤에서부터 찾는다.

```java
public void checkIndex() {
        String text = "My name is Juseon.";
        System.out.println(text.indexOf('s')); //9
        System.out.println(text.indexOf(" J")); //10
        System.out.println(text.indexOf('s', 11)); //13
}
```

<br>

**String에서 값의 일부를 추출**

**charAt()**

특정 위치의 char 값을 리턴한다.

```java
public void checkCharAt(){
        String text = "Ju Seon";
        System.out.println(text.charAt(3)); //3
}
```
<br>

**substring()**

대상 문자열을 잘라 String으로 리턴한다.

```java
public void checkSubString(){
        String text = "Ju Seon";
        String sub = text.substring(3);
        System.out.println(sub); //인덱스 3부터 자른다. Seon
}
```
<br>

**String에서 공백을 제거**

**trim()**

대상 문자열의 맨 앞과 뒤에 있는 공백들을 제거한 문자열을 리턴한다. 

```java
public void checkTrim(){
        String text = "  My name is  ";
        String result = text.trim();
        System.out.println(text);
        System.out.println(result);
}
```

<br>


**String은 불변 객체다**

String은 한 번 만들어지면 변경될 수 없다. 문자열을 더하게 되면 새로운 String 객체가 생성되고, 기존 객체는 버려진다. 그리고 GC의 대상이 된다.

이러한 String 클래스의 단점을 보완하기 위한 StringBuffer와 StringBuilder가 있다. StringBuffer는 Thread safe하며, StringBuilder는 Thread safe하지 않다. 속도는 StringBuilder 클래스가 빠르다. 두 클래스는 문자열을 더할 시에 새로운 객체를 생성하지 않는다.

  **그럼 언제 String을 사용할까?**

  불변성을 가지기 때문에 변하지 않는 문자열을 자주 읽는 경우 사용한다. 문자열 추가와 수정 삭제 등의 연산이 빈번하게 발생하는 경우 String을 사용하게 되면 힙에 많은 가비지가 생성되므로 StringBuffer / StringBuilder 클래스를 사용하는게 좋다.

<br>


참고 https://docs.oracle.com/javase/8/docs/api/java/lang/String.html
