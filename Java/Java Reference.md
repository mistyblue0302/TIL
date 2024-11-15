Java의 가비지 컬렉터(Garbage Collector)는 다양한 종류가 있지만 다음과 같은 작업을 공통적으로 수행합니다.

- Heap 내의 객체 중에서 더 이상 필요 없게 된 객체를 찾아낸다.
- 찾아낸 객체를 처리하여 Heap의 메모리를 회수한다.

초기 Java는 가비지 컬렉션(GC) 작업이 애플리케이션 코드와 독립적으로 수행하도록 설계되었습니다. 하지만 객체를 다양한 방법으로 처리하기 위해 JDK 1.2부터 `java.lang.ref` 패키지가 도입되어 사용자 코드와 GC와의 제한적인 상호작용을 할 수 있게 하고 있습니다.

Java의 기본 참조 유형인 `strong reference` 외에도 `java.lang.ref` 패키지는 `soft`, `weak`, `phantom` 3가지의 새로운 참조 방식을 각각의 `Reference` 클래스로 제공합니다. 이 3가지 `Reference` 클래스를 사용하면 GC에 일정 부분 관여할 수 있습니다.

## GC의 Reachability

GC는 객체가 가비지인지 판별하기 위해 `reachability`라는 개념을 사용합니다. 어떤 객체에 유효한 참조가 있다면 `reachable`, 없으면 `unreachable`로 구별하고 `unreachable`한 객체를 가비지로 간주 및 GC를 수행합니다. 하나의 객체는 여러 다른 객체를 참조하고, 참조된 다른 객체들도 또 다른 객체들을 참조할 수 있으므로 객체는 참조 사슬을 이룹니다. 여기서 Root Space은 항상 유효한 최초의 참조로, Root Space에서 시작해 객체가 참조 가능한지 여부를 검사합니다.

JVM의 Runtime Data Area의 구조를 보면 다음과 같습니다.

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/Reference.png)

Runtime Data Area는 쓰레드가 차지하는 영역과 객체를 생성 및 저장하는 Heap, 클래스 정보를 저장하는 Method Area로 나눌 수 있습니다. 위 구조에서 참조는 화살표로 표시되어 있습니다.

Heap에있는 객체들에 대한 참조 방식은 다음 네 가지 종류로 나눌 수 있습니다.

- Heap 내의 다른 객체에 의한 참조
- Java 스택의 참조(Java 메소드 실행 시 사용하는 로컬 변수나 파라미터들에 의한 참조)
- 네이티브 스택의 참조
- Method Area의 정적 변수에 의한 참조

이것들 중 Heap 내의 다른 객체에 의한 참조를 제외한 나머지 3개가 `reachability` 여부를 결정하는 기준이 됩니다. 

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/Reference2.png)

Root Space로부터 시작한 참조에 속한 객체들은 `reachable` 객체이고, 이 참조 사슬과 무관한 객체들은 `unreachable` 객체로 GC 대상이 됩니다. 만약 오른쪽 아래처럼 `reachable` 객체를 참조하더라도, 다른 `reachable` 객체가 이 객체를 참조하지 않는다면 `unreachable` 객체가 됩니다.

위 그림에서의 참조는 일반적인 참조로, `strong reference`라고 부릅니다.

## Soft, Weak, Phantom Reference

`java.lang.ref` 패키지는 `soft reference`, `weak reference`, `phantom reference`를 클래스 형태로 제공하여 `java.lang.ref.WeakReference` 클래스는 객체를 캡슐화한 `WeakReference` 객체를 생성합니다. 이렇게 생성된 객체는 다른 객체와 달리 GC가 특별하게 처리할 수 있게 합니다.

아래는 `WeakReference` 클래스가 객체를 생성하는 예시입니다.

~~~java
WeakReference<Sample> wr = new WeakReference<Sample>(new Sample());  
Sample ex = wr.get();  
ex = null; 
~~~

위 코드의 동작 방식은 다음과 같습니다.

`WeakReference` 객체는 `new Sample()`로 생성된 `Sample` 객체를 캡슐화한 객체입니다. 참조된 `Sample` 객체는 `wr.get()`을 통해 다른 참조를 통해 연결되고 이때 `WeakReference` 객체의 참조와 `ex` 참조, 두 개의 참조가 처음 생성한 `Sample` 객체를 가리킵니다.

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/Reference3.png)

코드 마지막 줄에서 `ex = null;`이 실행되면 처음에 생성한 `Sample` 객체는 오직 `WeakReference` 내부에서만 참조되고 이 객체는 `weakly reachable` 객체가 됩니다.

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/Reference4.png)

## java.lang.ref 패키지

원래 GC 대상 여부를 확인할 때는 `reachable`, `unreachable` 인가로만 구분하고 사용자 코드에서는 관여할 수 없었습니다. 하지만 `java.lang.ref` 패키지를 이용하여 `reachable` 객체들을 `strongly reachable`, `softly reachable`, `weakly reachable`, `phantomly reachable`로 더 자세히 구별하여 GC 때의 동작을 다르게 지정할 수 있습니다. 요약하여 GC 대상 여부를 판별할 때 사용자 코드도 관여할 수 있게 되었습니다. 위 그림에서 몇몇 객체들을 `WeakReference`로 바꾸면 아래와 같습니다.

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/Reference5.png)

녹색으로 표시된 두 객체는 `WeakReference`로만 참조된 `weakly reachable` 객체이고, 파란색 객체는 `strongly reachable` 객체입니다. GC가 동작하면 `unreachable` 객체뿐만 아니라 `weakly reachable` 객체도 가비지 객체로 간주되어 메모리에서 회수됩니다. Root Space로부터 시작된 참조 사슬에 포함이 되어있는데도 불구하고 GC가 동작할 때 메모리가 회수되기 때문에 참조는 가능하지만 항상 유효할 필요는 없는 LRU 캐시와 같은 임시 객체를 저장하는 구조를 쉽게 만들 수 있습니다. LRU 캐시는 자주 사용한 객체는 계속 보관하고 덜 사용한 객체는 제거하는 방식으로, 약한 참조를 통해 LRU 캐시를 구성하면 더 이상 사용되지 않는 객체는 자동으로 메모리에서 제거되기 때문에 메모리 사용을 최적화할 수 있습니다.

위 그림에서 `WeakReference` 객체 자체는 `weakly reachable` 객체가 아니라 `strongly reachable` 객체입니다. 또한, 그림에서 A로 표시된 객체와 같이 `WeakReference`에 의해 참조되고 있으면서 동시에 Root Space에서 시작한 참조 사슬에 포함되어 있는 경우에는 `weakly reachable` 객체가 아니라 `strongly reachable` 객체입니다.

GC가 동작하여 어떤 객체를 `weakly reachable` 객체로 판단하면, GC는 `WeakReference` 객체에 있는 `weakly reachable` 객체에 대한 참조를 null로 설정하여 `unreachable` 객체와 마찬가지인 상태로, 메모리 회수 대상이 됩니다.

## Strengths of Reachability

`reachability`는 총 다섯가지 종류가 있고 이는 GC가 객체를 처리하는 기준이 됩니다. 또한 하나의 객체는 `strong reference`, `soft reference`, `weak reference`, `phantom reference`의 다양한 조합으로 참조될 수 있습니다.

하나의 객체는 다음 5가지 `reachability` 중 하나가 될 수 있습니다.

- strongly reachable: Root Space에서 시작된 참조 사슬 중, 어떤 참조 객체(reference object)도 거치지 않고 직접적으로 참조 가능한 객체. 즉, 객체까지 도달하는 참조 경로 중 soft reference, weak reference, phantom reference가 없는 경로가 하나라도 있는 객체
- softly reachable: strongly reachable 객체가 아닌 객체 중에서 soft reference를 통해 도달 가능한 객체
- weakly reachable: strongly reachable 객체도 softly reachable 객체도 아닌 객체 중에서, phantom reference 없이 weak reference만 통과하는 참조 사슬이 하나라도 있는 객체
  phantomly reachable: strongly reachable 객체, softly reachable 객체, weakly reachable 객체 모두 해당되지 않는 객체. 이 객체는 파이널라이즈(finalize)되었지만 아직 메모리가 회수되지 않은 상태
- unreachable: Root Space에서 시작하는 참조 사슬로 참조되지 않는 객체

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/Reference6.png)

위의 경우 객체 B의 `reachability`는 `softly reachable`입니다. Root Space로부터 바로 `SoftReference`를 통해 B를 참조할 수 있기 때문입니다. 만약 SoftReference 참조가 없다면 객체 B는 `phantomly reachable`이 됩니다.

## Strong References

~~~java
public class ClassStrong {

    public static class Referred {
        protected void finalize() {
            System.out.println("method : finalize");
        }
    }

    public static void collect() throws InterruptedException {
        System.out.println("Suggesting collection");
        System.gc();
        System.out.println("Sleeping");
        Thread.sleep(5000);
    }

    public static void main(String args[]) throws InterruptedException {
        System.out.println("Creating strong references");
        Referred strong = new Referred();

        ClassStrong.collect();

        System.out.println("Removing reference");
        strong = null;
        ClassStrong.collect();

        System.out.println("Done");
    }
}
~~~

~~~java
Creating strong references
Suggesting collection
Sleeping
Removing reference
Suggesting collection
Sleeping
method : finalize
Done
~~~

`finalize()` 메소드는 객체가 GC에 의해 수거될 때 호출되는 메소드로 강한 참조가 유지될 때는 해당 메소드가 호출되지 않습니다. 하지만 이후에 `strong = null;`로 강한 참조를 제거한 후 `System.gc()`를 호출하면 `finalize()` 메소드가 호출되면서 객체가 수거됩니다.

## Soft references

이 경우 JVM은 **메모리가 부족할 때**만 GC에 의해 수거되도록 합니다.

~~~java
public class ClassSoft {

    public static class Referred {
        protected void finalize() {
            System.out.println("method : finalize");
        }
    }

    public static void collect() throws InterruptedException {
        System.out.println("Suggesting collection");
        System.gc();
        System.out.println("Sleeping");
        Thread.sleep(5000);
    }

    public static void main(String args[]) throws InterruptedException {
        System.out.println("Creating soft references");
        Referred strong = new Referred();

        SoftReference<Referred> soft = new SoftReference<Referred>(strong);

        ClassSoft.collect();

        System.out.println("Removing reference");
        strong = null;
        ClassSoft.collect();

        System.out.println("Consuming heap");
        try {
            List<ClassSoft> heap = new ArrayList<ClassSoft>(100000);
            while (true) {
                heap.add(new ClassSoft());
            }
        } catch (OutOfMemoryError e) {
            System.out.println("Out of memory error raised");
        }
        System.out.println("Done");
    }
}
~~~

~~~java
Creating soft references
Suggesting collection
Sleeping
Removing reference
Suggesting collection
Sleeping
Consuming heap
method : finalize
Out of memory error raised
Done
~~~

## Weak references

명시적으로 `weak reference`를 사용함으로써 해당객체가 GC 되도록 유도할 수 있습니다.

~~~java
public class ClassWeak {
    public static class Referred {
        protected void finalize() {
            System.out.println("method : finalize");
        }
    }

    public static void collect() throws InterruptedException {
        System.out.println("Suggesting collection");
        System.gc();
        System.out.println("Sleeping");
        Thread.sleep(5000);
    }

    public static void main(String args[]) throws InterruptedException {
        System.out.println("Creating weak references");
        Referred strong = new Referred();

        WeakReference<Referred> weak = new WeakReference<Referred>(strong);

        ClassWeak.collect();

        System.out.println("Removing reference");
        strong = null;
        ClassWeak.collect();

        System.out.println("Done");
    }
}
~~~

~~~java
Creating weak references
Suggesting collection
Sleeping
Removing reference
Suggesting collection
Sleeping
method : finalize
Done
~~~

## Phantom references

`Phantom references` 전에 `java.lang.ref` 패키지에서 제공하는 `ReferenceQueue` 클래스가 있습니다. `ReferenceQueue`는 참조 객체가 GC에 의해 수거될 때 해당 참조 객체를 큐에 넣는 역할을 합니다. 이렇게 하면 참조된 객체가 가비지 컬렉션에 의해 수거된 후, 개발자가 후속 작업을 실행할 수 있도록 합니다.

- 주요 동작 흐름
  - `SoftReference`, `WeakReference` 또는 `PhantomReference`로 참조된 객체가 가비지 컬렉션에 의해 수거됩니다.
  - 참조된 객체가 수거되면 참조 객체의 참조는 null로 설정됩니다.
  - 그 후, 해당 참조 객체가 `ReferenceQueue`에 enqueue됩니다. 이 작업은 자동으로 GC에 의해 수행됩니다.
  - 개발자는 `ReferenceQueue`에서 객체를 확인하고, 객체가 수거된 후 후속 작업(예: 리소스 해제, 캐시 갱신 등)을 처리할 수 있습니다.
 
`SoftReference`, `WeakReference`는 `ReferenceQueue`를 사용할 수도 있고 사용하지 않을 수도 있지만 `PhantomReference`는 반드시 `ReferenceQueue`를 사용해야 합니다. `PhantomReference`의 생성자는 단 하나이고 항상 `ReferenceQueue`를 인자로 받습니다.

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/Reference7.png)

`SoftReference`와 `WeakReference`는 객체가 수거될 때 참조가 null로 설정되면 그 객체를 `ReferenceQueue`에 넣지만 `PhantomReference`는 객체 내부의 참조를 null로 설정하지 않고 참조된 객체를 `phantomly reachable` 객체로 만든 이후에 `ReferenceQueue`에 enqueue 됩니다. 그래서 이후에 애플리케이션은 객체의 finalize() 이후에 필요한 작업들을 처리할 수 있습니다.

~~~java
public class ClassPhantom {

    public static class Referred { // 객체가 수거되기 전 필요한 작업을 처리할 수 있게 해주는 클래스
    }

    public static void collect() throws InterruptedException {
        System.out.println("Suggesting collection");
        System.gc();
        System.out.println("Sleeping");
        Thread.sleep(5000);
    }

    public static void main(String args[]) throws InterruptedException {
        System.out.println("Creating phantom references");
        ReferenceQueue queue = new ReferenceQueue();

        Map<Reference,String> cleanUpMap = new HashMap<Reference,String>();

        Referred strong = new Referred();

        PhantomReference<Referred> phantom = new PhantomReference(strong, queue);
        cleanUpMap.put(phantom, "You need to clean up some resources");

        strong = null;
        ClassPhantom.collect();

        Reference reference = queue.poll();
        if (reference != null) {
            System.out.println(cleanUpMap.remove(reference));
        }
        System.out.println("Done");
    }
}
~~~

~~~java
Creating phantom references
Suggesting collection
Sleeping
You need to clean up some resources
Done
~~~

<br>

참고

[https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/ref/package-summary.html](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/ref/package-summary.html)

[https://d2.naver.com/helloworld/329631](https://d2.naver.com/helloworld/329631)

[http://neverfear.org/blog/view/150/Strong_Soft_Weak_and_Phantom_References_Java](http://neverfear.org/blog/view/150/Strong_Soft_Weak_and_Phantom_References_Java)
