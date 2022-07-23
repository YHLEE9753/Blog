# Generics(3) - Generic 타입과 네이밍
2022/07/23

_위 글은 Oracle Java document - tutorial - genetics 공식 문서를 바탕으로 작성되었습니다.
다음 예시는 사양 및 구현이 완전하지만 영구적이지 않은 기능입니다. 향후 Java SE 릴리스에서 다른 형식으로 존재할 수도 있고 전혀 없을 수도 있습니다_

# 1. Generic 타입
- Non-generic Box class

```java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```

- generic Box class
```java
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

# 2. Type Parameter Naming Conventions

가장 많이 쓰이는 type parameter 이름들 이다.

- E - Element (used extensively by the Java Collections Framework)
- K - Key
- N - Number
- T - Type
- V - Value
- S,U,V etc. - 2nd, 3rd, 4th types


> 많은 사람들은 Type Parameter 과 Type Argument 을 혼용하여 사용한다. 하지만 실제로 2가지는 다른 의미를 가진다.

_When coding, one provides type arguments in order to create a parameterized type_

type parameter : `T` in `Foo<T>` f
type argument : `String` in `Foo<String>` f

## Multiple Type Parameters
```java
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
}
```

## Parameterized Types
type parameter (`K`,`V`) 는 parameterized type 인 (`List<String>`) 으로 대체 가능하다.
`OrderedPair<K, V>` 예시:
```java
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>(...))
```

## 참고 사이트
https://docs.oracle.com/javase/tutorial/java/generics/types.html



