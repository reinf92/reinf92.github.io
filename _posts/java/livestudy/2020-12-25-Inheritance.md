---
title: "[Live Study] 6주차 과제 : 상속"
date: 2020-12-25 22:00:00 +0900
categories: [Java, Live Study]
tags: [java, inheritance]
---

[![study-halle](/assets/img/posts/java/livestudy/study-halle.png)](https://github.com/whiteship/live-study)

이 스터디는 백기선님과 시청자분들이 함께하는 온라인 자바 스터디입니다.

- [https://www.youtube.com/channel/UCwjaZf1WggZdbczi36bWlBA](https://www.youtube.com/channel/UCwjaZf1WggZdbczi36bWlBA)
- [https://github.com/whiteship/live-study](https://github.com/whiteship/live-study)

<br/>

## 자바 상속의 특징

자바에서 상속이란 `부모 클래스에서 정의된 필드와 메소드를 자식 클래스가 물려받는 것`이다.

### 상속의 필요성?

- 공통된 특징을 가지는 클래스 사이의 멤버(필드, 메소드) 선언이 불필요하다.
- 부모 클래스의 멤버(필드, 메소드)를 재사용함으로써 자식 클래스가 간결해진다.
- 클래스간 계층적 분류 및 관리가 쉬워진다.

### 상속의 특징

- 다중 상속을 지원하지 않는다. `extends` 뒤에는 단 하나의 부모 클래스만 올 수 있다.
- 상속의 횟수에 제한이 없다.
- 최상위 클래스의 존재, Object 클래스
  - 모든 클래스는 Object 클래스의 자식 클래스이다.

![inheritance](/assets/img/posts/java/livestudy/week06/inheritance.png)
_출처: [https://kephilab.tistory.com/56](https://kephilab.tistory.com/56)_

그림처럼 부모 클래스를 상속 받는 자식 클래스는 부모 클래스에서 정의한 멤버(필드, 메소드)를 별도로 정의하지 않더라도 사용할 수 있다.

그에 따라 부모 클래스로 부터 확장된 자식 클래스를 불필요한 코드 중복이 발생하지 않게 사용할 수 있다.

### 상속의 선언

`extends` 키워드를 이용하여 아래와 같이 선언한다.

```java
//부모클래스
public class Parent{
    ....
}
//Parent를 상속받는 자식클래스 Child
public class Child extends Parent{
    ....
}
```

## super 키워드

지난시간 `this`는 자기 자신의 참조 키워드였다. super도 이와 비슷하다. `super`는 `자식 클래스에서 부모 클래스를 참조`하는 키워드이다. 또한 super()는 부모 클래스의 생성자를 의미한다.

![super](/assets/img/posts/java/livestudy/week06/super.png)

자식 클래스를 생성하면 그림과 같은 구조로 메모리에 데이터가 올라간다.

자식 객체를 생성하면 부모 객체도 생성되며 자식 객체는 super 키워드를 이용하여 부모 객체의 주소를 참조하게된다. 그리고 이 super 키워드를 이용하여 부모 클래스의 멤버를 사용할 수 있는 것이다.

부모 클래스의 멤버를 물려 받는다는 것은 엄밀히말하면 물려받는 것은 아니고 super 키워드를 이용하여 생성된 부모 객체의 멤버를 사용하는 것이라고 할 수 있다.

```java
package week06;

class ParentB {
    int pValue = 10;

    public ParentB() {
        System.out.println("ParentB 생성자 호출");
    }
}

class ChildB extends ParentB {
    int cValue = 20;

    public ChildB() {
        // super(); 생략
        // super() == ParentB()
        System.out.println("ChildB 생성자 호출");
    }
}

public class Inheritance {

    public static void main(String[] args) {
        // ParentB 생성자 호출
        // ChildB 생성자 호출
        ChildB child = new ChildB();
        System.out.println(child.pValue);   // 10
        System.out.println(child.cValue);   // 20
    }
}
```

## 메소드 오버라이딩

메소드 오버라이딩은 부모 클래스에 정의된 메소드를 자식 클래스에서 재정의하여 사용하는 것을 말한다.

- 오버라딩의 조건

  - 메소드의 선언부는 기존 메소드와 완전히 같아야 한다.
  - 메소드 반환 타입은 부모 클래스의 반환 타입으로 타입 변환할 수 있는 타입이라면 변경할 수 있다.
  - 부모 클래스의 메소드보다 접근 제어자를 더 좁은 범위로 변경할 수 없다.
  - 부모 클래스의 메소드보다 더 큰 범위의 예외를 선언할 수 없다.

```java
package week06;

class ParentC {
    public void print(){
        System.out.println("Parent의 print메소드 호출");
    }
}

class ChildC extends ParentC {
    @Override
    public void print() {
        System.out.println("Child의 print메소드 호출");
    }
}

public class MethodOverriding {

    public static void main(String[] args) {
        ParentC a = new ParentC();
        ParentC b = new ChildC();
        ChildC c = new ChildC();

        a.print();  // Parent의 print메소드 호출
        b.print();  // Child의 print메소드 호출
        c.print();  // Child의 print메소드 호출
    }
}
```

## 메소드 디스패치(Method Dispatch)

메소드 디스패치는 어떤 메소드를 호출할지 결정하여 실행시키는 과정을 말한다. 이 과정에는 정적 디스패치(Static Dispatch), 동적 디스패치(Dynamic Dispatch) 두 가지 경우가 존재한다.

### 정적 디스패치(Static Dispatch)

컴파일 시점에서, 컴파일러가 특정 메소드를 호출할 것이라고 명확하게 알고있는 경우이다.

```java
package week06;

public class StaticDispatch {

    static class Service {
        void print() {
            System.out.println("Service.print");
        }
    }

    public static void main(String[] args) {
        // Service 클래스의 print() 메소드를 호출할 것임이 명확
        Service service = new Service();
        service.print();    // Service.print
    }
}
```

### 동적 디스패치(Dymamic Dispatch)

정적 디스패치와 다르게 컴파일러가 어떤 메소드를 호출하는지 모르는 경우이다. 동적 디스패치는 호출할 메소드를 런타임 시점에서 결정하게 된다.

```java
package week06;

public class DymamicDispatch {

    static abstract class Service {
        abstract void print();
    }

    static class ServiceImpl1 extends Service {
        @Override
        public void print() {
            System.out.println("ServiceImpl1.print");
        }
    }

    static class ServiceImpl2 extends Service {
        @Override
        public void print() {
            System.out.println("ServiceImpl2.print");
        }
    }

    public static void main(String[] args) {
        // Service 인터페이스의 print() 메소드를 호출할 경우 구현체에 따라 다름
        // 컴파일 시점에는 어떤 메소드를 호출할지 모르기 떄문에 런타임시점에 결정
        Service service = new ServiceImpl1();
        service.print();    // ServiceImpl1.print
    }
}
```

## 추상 클래스

클래스와 객체간의 관계에서 클래스가 설계도라고 한다면 추상 클래스는 미완성된 설계도라고 할 수 있다.

추상 클래스에서의 미완성이라는 것은 모든게 미완성 상태인 것은 아니고, 미완성 메소드(추상 메소드)를 포함한 경우를 의미한다.

상속관계에서 공통적으로 사용되는 기능(메소드)를 구현하지 않고 추상화하여 목적만 명시하고 기능구현은 하위 클래스에서 하는 방식으로 사용한다.

- 자기 자신의 인스턴스 생성이 불가하다.
- 추상 메소드를 포함하고 있다는 것을 제외하면 일반 클래스와 동일하다.
- 생성자와 멤버 변수, 일반 메서드를 모두 가질 수 있다.

```java
abstract class 클래스이름 {
    abstract void method();
    ...
}
```

객체지향 프로그래밍에서는 상속 계층도를 따라 내려갈 수록 클래스는 기능이 점점 추가되어 구체와의 정도가 커지며, 반대로 올라갈수록 추상화의 정도가 커진다고 할 수 있다.

즉, 상승계층도를 따라 올라갈 수록 공통적인 요소만 남게된다.

- 추상화 : 클래스간의 공통점을 찾아내어 공통기능을 상위 클래스 또는 인터페이스로 만드는 작업
- 구체화 : 상속 또는 구현을 통해 클래스를 구현하고 기능을 확장하는 작업

## final 키워드

final은 `변경될 수 없다`는 의미를 가지고 있으며, 거의 모든 대상에서 사용할 수 있다.

- 클래스 : 변경될 수 없는 클래스, `상속 불가`
- 메소드 : 변경될 수 없는 메소드, `오버라이딩 불가`
- 변수 : 변경될 수 없는 값, `상수`

## Object 클래스

**상속계층도의 최상위에 있는 클래스로**서 다른 클래스로부터 상속을 받지 않는 모든 클래스는 별도로 명시하지 않더라도 Object 클래스를 상속받는다.

```java
class Child {
    ...
}

class Child extends Object {
    ...
}
```

Child 클래스는 `컴파일 시점에 자동으로 extends Object를 추가하여 상속`받게 된다.

### Object 클래스의 메소드

| 메소드                            | 설 명                                                                                   |
| --------------------------------- | --------------------------------------------------------------------------------------- |
| protected Object clone()          | 객체 자신의 복사본을 반환                                                               |
| public boolean equals(Object obj) | 객체 자신과 파라미터로 전달받은 객체가 동일한 객체인지 여부                             |
| protected void finalize()         | 객체가 소멸될 때 가비지 컬렉터에 의해 자동으로 호출                                     |
| public Class getClass()           | 객체 자신의 클래스 정보를 담고 있는 클래스 인스턴스를 반환                              |
| public int hashCode()             | 객체 자신의 해시코드를 반환                                                             |
| public String toString()          | 객체 자신의 정보를 문자열로 반환                                                        |
| public void notify()              | 대기중인 쓰레드 중 임의로 하나를 깨운다                                                 |
| public void notifyAll()           | 대기중인 쓰레드를 모두 깨운다                                                           |
| public void wait()                | 다른 쓰레드가 notify() 또는 notifyAll()을 호출할 때까지 현재 쓰레드를 대기상태로 만든다 |

Object 메소드에 대한 자세한 내용은 [이곳](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)을 참조
