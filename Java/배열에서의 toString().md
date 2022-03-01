# 💡 **배열**

배열은 값을 연속적인 메모리 위치에 저장하고 있다.

```java
import java.util.Arrays;

public class arraytest {

	public static void main(String[] args) {
		int[] array = new int[] {1,2,3,4};

		System.out.println(array.toString());
		System.out.println(Arrays.toString(array));
	}

}

```

```java
> [I@43a25848] //Object.toString()메소드의 값을 출력하는 것이고, 대상 개체의 해쉬코드값을 출력한다
> [1, 2, 3, 4] //java.util.Araays 패키지를 이용하여 값을 문자열 형태로 리턴
```
