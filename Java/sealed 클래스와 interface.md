# sealed 클래스와 interface
2022/07/23

_위 글은 Java SE 15 update - Record 공식 문서를 바탕으로 작성되었습니다._
_다음 예시는 사양 및 구현이 완전하지만 영구적이지 않은 기능입니다. 향후 Java SE 릴리스에서 다른 형식으로 존재할 수도 있고 전혀 없을 수도 있습니다_

## 1. sealed 클래스
Sealed classes와 interfaces는 다른 classes나 interfaces가 extend 하거나 implement 할 수 있는 것을 제한합니다.


상속의 주요 목적 중 하나는 코드 재사용입니다.
하지만 entity 를 정의하고 도메인 내에서 존재하는 다양한 가능성들을 modeling 하는 경우는 어떻게 해야 할까요?
예를 들어 Shape 이라는 클래스를 만들었는데 임의의 클래스가 Shape 를 extend 하는 것을 원치 않을 수도 있다. 이런 경우 class 를 sealing 함으로써, 어느 클래스를 extend 하도록 허용하고, 어느 임의의 클래스는 막을 것인지 설정할 수 있다.

## 2. Defining Sealed classes
- class 를 seal 하기 위해서 `sealed` modifier 을 선언문에 추가해야 한다.
- 그 후 `extends` 혹은 `implements` 를 적는다.
- 그 후 `permits` 을 추가한다.
- `permits` 은 seaded class 를 extend 하는데 specified 된 클래스들이다.

예를 들어 `Shape` 클래스를 선언하는데 3개의 허용된 subclasses 들은 다음과 같다. : `Circle`, `Square`, `Rectangle`
```java
public sealed class Shape
    permits Circle, Square, Rectangle {
}
```

동일 module, 혹은 같은 package 에 있을 때 Circle, square, Rectangle 클래스를 사용 할 수 있다.
```java
public final class Circle extends Shape {
    public float radius;
}
```

```java
public final class Circle extends Shape {
    public float radius;
}
```

```java
public sealed class Rectangle extends Shape permits FilledRectangle {
    public double length, width;
}
```

```java
public final class FilledRectangle extends Rectangle {
    public int red, green, blue;
}
```

또는  sealed class와 동일한 파일에 허용된 subclasses 를 정의할 수 있습니다. 그렇게 하면 다음 permits 절을 생략할 수 있습니다
```java
package com.example.geometry;

public sealed class Figure
    // The permits clause has been omitted
    // as its permitted classes have been
    // defined in the same file.
{ }

final class Circle extends Figure {
    float radius;
}
non-sealed class Square extends Figure {
    float side;
}
sealed class Rectangle extends Figure {
    float length, width;
}
final class FilledRectangle extends Rectangle {
    int red, green, blue;
}

```

## 3. Permitted Subclasses 에서의 제약

- `subclasses` 들은 `compile` 시점에 `sealed class` 에 의해 접근가능해야 한다.
    - `Shape.java` 를 컴파일 하기 위해 `Shape` 의 permitted 된 classes 들인 `Circle.java`, `Square.java`, `Rectangel.java` 에 접근 가능 해야 한다.
    - 추가로 `Rectangle` 또한 sealed class 이기 때문에 compiler 는 추가로 `FilledRectangle.java` 에 접근 가능 해야 한다.
- 반드시 다음중 하나의 modifiers 를 가져야 한다.(describe how it continues the sealing initiated by its superclass)
    - `final` : 더이상 확장 불가능
    - `sealed` : 오직 permiitted subclasses 만 확장 가능
    - `non-sealed` : 아무 subclasses 에 의해 확장 가능
- subclasses 들은 반드시 sealed class 와 같은 module 에 있어야 한다.
- 혹은 같은 package 에 있어야 한다.


## 4. Sealed Interfaces 정의
sealed 클래스와 마찬가지로 interface 를 seal 하기 위해서 modifier 를 선언시 추가한다. 그 후 sealed interface 를 확장할 수 있는 `interface` 를 지정하는 `permits` 를 `extends` 다음에 추가한다.
```java
package com.example.expressions;

public class TestExpressions {
  public static void main(String[] args) {
    // (6 + 7) * -8
    System.out.println(
      new TimesExpr(
        new PlusExpr(new ConstantExpr(6), new ConstantExpr(7)),
        new NegExpr(new ConstantExpr(8))
      ).eval());
   }
}

sealed interface Expr
    permits ConstantExpr, PlusExpr, TimesExpr, NegExpr {
    public int eval();
}

final class ConstantExpr implements Expr {
    int i;
    ConstantExpr(int i) { this.i = i; }
    public int eval() { return i; }
}

final class PlusExpr implements Expr {
    Expr a, b;
    PlusExpr(Expr a, Expr b) { this.a = a; this.b = b; }
    public int eval() { return a.eval() + b.eval(); }
}

final class TimesExpr implements Expr {
    Expr a, b;
    TimesExpr(Expr a, Expr b) { this.a = a; this.b = b; }
    public int eval() { return a.eval() * b.eval(); }
}

final class NegExpr implements Expr {
    Expr e;
    NegExpr(Expr e) { this.e = e; }
    public int eval() { return -e.eval(); }
}
```

## 5. Record Classes as Permitted Subclasses
`sealed class` 혹은 `sealed interface` 의 `permits` 절에서 `record class` 를 이름지을 수 있다.
record 클래스는 implicitly 하게 final 이다. 따라서 위의 에시를 일반 class 대신에 record 클래스로 대체할 수 있다.

```java
package com.example.records.expressions;

public class TestExpressions {
  public static void main(String[] args) {
    // (6 + 7) * -8
    System.out.println(
      new TimesExpr(
        new PlusExpr(new ConstantExpr(6), new ConstantExpr(7)),
        new NegExpr(new ConstantExpr(8))
      ).eval());
   }
}

sealed interface Expr
    permits ConstantExpr, PlusExpr, TimesExpr, NegExpr {
    public int eval();
}

record ConstantExpr(int i) implements Expr {
    public int eval() { return i(); }
}

record PlusExpr(Expr a, Expr b) implements Expr {
    public int eval() { return a.eval() + b.eval(); }
}

record TimesExpr(Expr a, Expr b) implements Expr {
    public int eval() { return a.eval() * b.eval(); }
}

record NegExpr(Expr e) implements Expr {
    public int eval() { return -e.eval(); }
}
```

## 참고 사이트
https://docs.oracle.com/en/java/javase/15/language/sealed-classes-and-interfaces.html