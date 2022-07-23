# Generics(4) - Raw Type 과 type inference
2022/07/23

_위 글은 Oracle Java document - tutorial - genetics 공식 문서를 바탕으로 작성되었습니다.
다음 예시는 사양 및 구현이 완전하지만 영구적이지 않은 기능입니다. 향후 Java SE 릴리스에서 다른 형식으로 존재할 수도 있고 전혀 없을 수도 있습니다_

# 1. Raw Type(원시타입)
매개변수화 타입(parameterized type) : `List<String>`
로 타입(raw type) : `List`

로타입은 제너릭이 도래하기 전 코드와 호환하기 위한 궁여지책이다.
따라서 기존 라이브러리를 변경하는 케이스가 아니면 반드시 로타입을 사용하면 안된다.
실제 로 타입을 사용하면 컴파일에서 감지하지 못하지만 런타입에서 감지하는 `ClassCaseException` 등의 에러를 마주하게 된다. 런타임에서 체킹 되기 때문에 실제 오류가 발생한 코드와 물리적인 거리가 존재하고 추적이 어려워지는 단점이 있다.

# 2. type inference

```java

public class Test {
    Pair<Integer, String> p1 = new Pair<>(1, "apple");
    Pair<Integer, String> p2 = new Pair<>(2, "pear");
    boolean same1 = Util.<Integer, String>compare(p1, p2);
    // <Integer, String> 은 없어도 되는 코드이다.

    Pair<Integer, String> p3 = new Pair<>(1, "apple");
    Pair<Integer, String> p4 = new Pair<>(2, "pear");
    boolean same2 = Util.compare(p3, p4);
    // 간결해진 코드
}


class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
                p1.getValue().equals(p2.getValue());
    }
}

class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}

```

- 위 코드와 같이 타입은 생략 가능하며, 컴파일러가 필요한 타입을 추론합니다.
- 이러한 특징은 type inference 라고 합니다.

## 참고 사이트 및 도서
이펙티브 자바 - 조슈아 블로크
https://docs.oracle.com/javase/tutorial/java/generics/methods.html
https://docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html


