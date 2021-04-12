---
title: "[Live Study] 8주차 과제 : 인터페이스"
date: 2021-01-08 22:00:00 +0900
categories: [Java, Live Study]
tags: [java, interface]
---

[![study-halle](/assets/img/posts/java/livestudy/study-halle.png)](https://github.com/whiteship/live-study)

이 스터디는 백기선님과 시청자분들이 함께하는 온라인 자바 스터디입니다.

- [https://www.youtube.com/channel/UCwjaZf1WggZdbczi36bWlBA](https://www.youtube.com/channel/UCwjaZf1WggZdbczi36bWlBA)
- [https://github.com/whiteship/live-study](https://github.com/whiteship/live-study)

<br/>

## 인터페이스

인터페이스는 추상 클래스와 같이 미완성된 설계도라고 할 수 있다.

추상 클래스와 마찬가지로 추상 메소드를 갖지만 추상 클래스보다 추상화의 정도가 높아, 일반 메소드나 멤버변수를 구성원으로 가질 수 없다. 오직 추상 메소드와 상수만을 멤버로 가질 수 있으며, 그 외의 어떤 요소도 허용되지 않는다.

또한, 클래스의 경우 다중 상속을 지원하지 않기 때문에 큰 제약이 있으나 인터페이스는 다중 상속을 지원한다.

### 인터페이스 사용의 장점

- 개발시간 단축
- 표준화 가능
- 클래스간의 결합도를 낮춤
- 독립적인 프로그래밍 가능

### 인터페이스 정의

인터페이스는 클래스를 정의하는 것과 동일하다.

다만 class 대신 interface를 사용한다.접근제어자는 `public`, `default`를 사용할 수 있다.

- 인터페이스는 공용 API를 정의하기에 접근제어자를 지정하지 않더라도 묵시적으로 `public` 선언이 된다.
- 인터페이스에서 필드는 상수만 제공하기 때문에 묵시적으로 `static final` 선언이 된다.
- 인터페이스는 인스턴스화 할 수 없기때문에 생성자가 필요하지 않다.

```java
interface 인터페이스명 {
    타입 상수명 = 값;
    타입 메소드명(매개변수, ...);
}
```

### 인터페이스 구현

인터페이스를 구현하는 클래스는 지난 상속에서 배운 추상 클래스와 비슷하지만 키워드가 다르다. 인터페이스를 구현한다는 의미에서 `implements` 키워드를 사용하여 구현한다.

또한 인터페이스를 구현하는 클래스의 경우 반드시 인터페이스에 정의되어 있는 추상 메소드를 구현해야 한다.

```java
package week08;

interface Animal {
    void cry();
}
```

```java
package week08;

class Cat implements Animal{
    @Override
    public void cry() {
        System.out.println("야옹");
    }
}
```

```java
package week08;

class Lion implements Animal{
    @Override
    public void cry() {
        System.out.println("으르렁");
    }
}
```

## 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법

다형성을 공부하면 자식 클래스의 인스턴스를 부모 타입으로 변환하는 것이 가능하다는 것을 알 수 있다.

인터페이스 또한 구현하는 클래스의 부모라고 할 수 있기 때문에 해당 인터페이스 타입의 참조 변수로 구현 클래스의 인스턴스를 참조할 수 있고, 타입 변환도 가능하다.

```java
package week08;

public interface Animal {
    void cry();
}
```

```java
package week08;

public class Cat implements Animal {
    @Override
    public void cry() {
        System.out.println("야옹");
    }

    public void action() {
        System.out.println("꾹꾹이");
    }
}
```

```java
package week08;

public class Lion implements Animal {
    @Override
    public void cry() {
        System.out.println("으르렁");
    }

    public void hunting(){
        System.out.println("사냥");
    }
}
```

```java
package week08;

public class App {
    public static void main(String[] args) {
        Animal cat = new Cat();
        Animal lion = new Lion();

        cat.cry();
        lion.cry();

        ((Cat)cat).action();
        ((Lion)lion).hunting();
    }
}
```

```plaintext
야옹
으르렁
꾹꾹이
사냥
```

## 인터페이스 상속

인터페이스는 다른 인터페이스를 상속받아 확장 할 수 있다. 인터페이스의 경우 클래스와 다르게 다중 상속이 가능하며, 인터페이스를 구현하는 구현 클래스는 다중 구현이 가능하다.

```java
package week08;

interface Mammal extends Animal, Comparable<Mammal> {
    void action();
    int getLegs();
}

//구현 클래스
public class Poppy implements Mammal{
    //Mammal 인터페이스의 메소드 구현
    @Override
    public void action() {
        System.out.println("네 발로 걷다.");
    }
    //Mammal 인터페이스의 메소드 구현
    @Override
    public int getLegs() {
        return 4;
    }
    //Comparable 인터페이스의 메소드 구현
    @Override
    public int compareTo(Mammal o) {
        return getLegs() - o.getLegs();
    }
    //Animal 인터페이스의 메소드 구현
    @Override
    public void cry() {
        System.out.println("멍멍");
    }
}
```

```plaintext
야옹
으르렁
꾹꾹이
사냥
```

![diagram](/assets/img/posts/java/livestudy/week08/diagram.png)

Puppy 클래스는 계층 구조에 따라 Animal, Mammal 타입으로 인스턴스화가 가능하다.

```java
package week08;

public class App2 {
    public static void main(String[] args) {
        Poppy poppy = new Poppy();
        Mammal mammal = poppy;
        Animal animal = mammal;

        poppy.cry();
        poppy.action();
        poppy.getLegs();

        mammal.cry();
        mammal.action();
        mammal.getLegs();

        animal.cry();
    }
}
```

```plaintext
멍멍
네 발로 걷다.
멍멍
네 발로 걷다.
멍멍
```

## 인터페이스의 Default Method, 자바 8

인터페이스는 기능에 대한 선언만 가능하고, 실제 구현은 구현 클래스에서 하기 때문에 실제 코드를 구현하지 않는다.하지만 자바8에서 인터페이스에서도 구현을 가능케 하는 기능이 나왔는데, 이것이 바로 디폴트 메소드 이다.

```java
interface Book {
    default void print(){
        System.out.println("Book default method");
    }
}
```

기존에 존재하던 인터페이스를 이용하여 여러 구현 클래스를 만들어 사용하고 있는 와중에 인터페이스를 보완하는 과정에 메소드를 추가해야 한다면

현재 인터페이스를 구현하고 있는 모든 클래스에 추가한 메소드를 구현해야하는 번거로운일이 발생한다. 이 때, default 메소드를 이용하여 호환성을 유지할 수 있다.

실제로 스프링 프레임 워크 버전 4에서 WebMvcConfigure라는 인터페이스를 구현한 클래스 WebMvcConfigurerAdapter를 사용하였지만 자바8에서 디폴트 메소드가 등장하고 WebMvcConfigurerAdapter클래스는 더 이상 사용되지 않고있다.

```java
interface Book {
    default void print(){
        System.out.println("Book default method");
    }
}

class MyBook implements Book{}

public class DefaultMethod {
    public static void main(String[] args) {
        Book book = new MyBook();
        book.print(); //Book default method 출력
    }
}
```

## 인터페이스의 Static Method, 자바 8

인스턴스 생성과 관계없이 인터페이스 타입으로 호출할 수 있는 메소드이다. static이기 때문에 `override가 불가`하다.

```java
interface Machine {
    static void print(){
        System.out.println("Machine static method!!");
    }
}

public class StaticMethod {
    public static void main(String[] args) {
        Machine.print(); //Machine static method!! 출력
    }
}
```

## 인터페이스의 Private Method, 자바 9

인터페이스의 메소드는 기본적으로 모두 `public` 이기 때문에 자바8에서 추가된 디폴트 메소드와 정적 메소드의 특정 기능을 처리하는 내부 메소드 또한 public으로 만들었어야 했다.

자바9에서는 이를 개선하기 위해서 `private method`와 `private static method`라는 새로운 기능을 제공해준다.

이를 통해 코드의 중복을 피하고 인터페이스에 대한 캡슐화를 유지할 수 있게 되었다.

```java
interface Car {
    default void print(){
        System.out.println("Name : " + getName());
        System.out.println("Price : " + getPrice());
    }

    private String getName(){
        return "테슬라";
    }

    private static Integer getPrice(){
        return 100000000;
    }
}

class MyCar implements Car{}

public class PrivateMethod {
    public static void main(String[] args) {
        Car car = new MyCar();
        car.print();
    }
}
```
