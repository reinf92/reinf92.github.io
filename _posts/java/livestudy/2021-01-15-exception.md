---
title: "[Live Study] 9주차 과제 : 예외 처리"
date: 2021-01-15 22:00:00 +0900
categories: [Java, Live Study]
tags: [java, exception]
---

[![study-halle](/assets/img/posts/java/livestudy/study-halle.png)](https://github.com/whiteship/live-study)

이 스터디는 백기선님과 시청자분들이 함께하는 온라인 자바 스터디입니다.

- [https://www.youtube.com/channel/UCwjaZf1WggZdbczi36bWlBA](https://www.youtube.com/channel/UCwjaZf1WggZdbczi36bWlBA)
- [https://github.com/whiteship/live-study](https://github.com/whiteship/live-study)

<br/>

## Error와 Exception의 차이는?

오류(Error)는 시스템에 비정상적인 상황이 생겼을 때 발생하게 된다. 이는 `시스템 레벨에서 발생`하기 때문에 심각한 수준의 오류이며, 개발자가 미리 예측하여 처리할 수 없기 때문에 애플리케이션에서 오류에 대한 처리를 신경 쓰지 않아도 된다.

오류가 시스템 레벨에서 발생한다면, `예외(Exception)는 개발자가 구현한 로직에서 발생`하게 된다. 예외는 `발생할 상황을 미리 예측하여 처리하는 것이 가능`하다.

## 자바에서 예외 처리 방법 (try, catch, finally, throw, throws)

### 예외 처리의 기본 구조 (try-catch-finally)

자바에서 예외 처리는 기본적으로 try-catch-finally 구조를 통해 처리하게 된다.

```java
try {
    ... // 수행 문장
} catch (Exception) {
    ... // 예외 처리
} finally {
    ... // 예외 여부와 관계없이 실행
}
```

- try문안의 수행할 문장들에서 예외가 발생하지 않는다면 catch문 다음의 문장들은 수행이 되지 않는다.
- 하지만, try문안의 문장들을 수행 중 해당 예외가 발생하면 예외에 해당되는 catch문이 수행된다.
- 다만 finally문안의 문장들은 예외발생 여부와 관계없이 반드시 실행된다.

예를 들어 숫자를 0으로 나눈다고 가정하면 다음과 같은 상황이 발생하게 된다.

```java
package week09;

public class TryCatchFinally {

    public static void main(String[] args) {
        int value = 0;

        try {
            value = 10 / 0;
        } catch (ArithmeticException e) {
            value = -1;
        } finally {
            System.out.println("예외 여부와 관계없이 반드시 실행");
        }
        System.out.println(value);
    }
}
```

```plaintext
예외 여부와 관계없이 반드시 실행
-1
```

10을 0으로 나누었기 때문에 `ArithmeticException`이 발생하게 되고, 해당 예외를 잡아 catch문이 수행되어 value 변수에 -1을 저장하여 출력된다.

그리고 finally문의 메시지는 예외발생 여부와 관계없이 출력되게 된다.

### 예외 발생 시키키 (throw)

`throw`는 인위적으로 예외를 발생시키기 위해서 사용되는 예약어로 개발자가 의도하는 케이스에 대해 임의로 예외를 발생시킬 때 사용된다.

특정 예외를 만났을 경우 더 구체적인 예외로 처리하고자 할 때에도 사용이 된다.

```java
package week09;

public class Throw {

    public static void main(String[] args) {
        try {
            // .. do something
            throw new Exception();
        } catch (Exception e) {
            throw new IllegalStateException("...");
        } finally {
            System.out.println("시스템 종료");
        }
    }
}
```

```plaintext
시스템 종료
Exception in thread "main" java.lang.IllegalStateException: ...
	at week09.Throw.main(Throw.java:10)
```

### 예외 던지기 (throws)

`thorws`는 예외가 발생한 메소드를 `호출한 곳으로 예외 객체를 넘기는 방법`으로, 예외가 발생한 지점에서 직접 예외 처리를 하지 않고 예외가 발생한 메소드를 호출한 지점으로 예외를 전달하여 처리할 때 사용된다.

```java
package week09;

public class Throws {

    public static void main(String[] args) {
        try {
            // 예외가 발생한 메소드를 호출한 지점에서 예외 처리
            ThrowsMethod();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void ThrowsMethod() throws Exception{
        // 예외 발생시 직접 예외처리를 하지 않음.
        throw new Exception("예외 발생시키기");
    }
}
```

```plaintext
java.lang.Exception: 예외 발생시키기
	at week09.Throws.ThrowsMethod(Throws.java:16)
	at week09.Throws.main(Throws.java:8)
```

## 자바가 제공하는 예외 계층 구조

자바에서 Exception은 여러 자식 클래스를 가지는데 예외 클래스들은 두 가지 그룹으로 나누어 질 수 있다.

바로 `Checked Exception`과 `Unchecked Exception`인데, 예외 클래스 중 `RuntimeException`클래스가 두 그룹을 구분하는 기준이 되는 클래스이다.

![exception-layer](/assets/img/posts/java/livestudy/week09/exception-layer.png)
_출처: [https://itmining.tistory.com/9](https://itmining.tistory.com/9)_

`RuntimeException`과 이를 상속받는 클래스들은 `Unchecked Exception`에 속하며 `Checked Exception`과는 조금 다르게 취급된다 명시적으로 예외 처리를 하지 않아도 된다는 점이다.

```java
package week09;

public class CheckedException {

    public static void main(String[] args) {
        try {
            // try-catch 예외 처치를 하지 않으면 컴파일 시점에서 걸린다.
            throw new Exception();
        } catch (Exception e) {
            System.out.println("Exception 발생");
        }
    }
}
```

`Checked Exception`에 속하는 Exception 예외 발생 시 컴파일 시점에 예외 처리를 강제하기 때문에 try-catch 문을 이용하여 `반드시 예외 처리`를 해야 한다.

```java
package week09;

public class UncheckedException {

    public static void main(String[] args) {
        // 컴파일 시점에 체크되지 않기 때문에 명시적으로 예외 처리를 하지 않아도 됨.
        throw new NullPointerException();
    }
}
```

`Uncecked Exception`에 속하는 NullPointerException 예외 발생 시 컴파일 시점에 체크가 되지 않기 때문에 `명시적으로 예외 처리를 하지 않아`도 된다.

|               | Checked Exception                                                    | Unchecked Exception                                                                                                |
| ------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| 확인시점      | 컴파일(Compile) 시점                                                 | 런타임(Runtime) 시점                                                                                               |
| 처리여부      | 반드시 명시적 예외 처리를 해야 함                                    | 명시적 예외처리를 강제하지 않음                                                                                    |
| 트랜잭션 처리 | 예외 발생 시 롤백 하지 않음                                          | 예외 발생 시 롤백 해야 함                                                                                          |
| 종류          | IOException <br/> SqlException <br/> ClassNotFoundException <br> ... | NullPointerException <br/> IllegalArgumentException <br/> IndexOutOfBoundException <br/> SystemException <br/> ... |

## 커스텀한 예외 만드는 방법

기본적으로 자바에서 제공되는 예외 클래스 이외에 필요에 따라 새로운 예외 클래스가 필요할 경우 커스텀 예외 클래스를 만들어 사용할 수 있다.

Exception 클래스 또는 필요에 따라 알맞은 예외 클래스를 상속받아 만들 수 있다.

```java
package week09;

class DivideException extends Exception {
    DivideException(){
        super();
    }

    DivideException(String message){
        super(message);
    }
}

public class ExceptionInheritance {

    static void divide(int lValue, int rValue){
        if (rValue == 0) {
            try {
                throw new DivideException("0으로 나누는 것은 허용되지 않습니다.");
            } catch (DivideException e) {
                e.printStackTrace();
            }
        } else {
            System.out.println(lValue / rValue);
        }
    }

    public static void main(String[] args) {
        divide(10, 5);  // 2
        divide(10, 0);  // DivideException 발생
    }
}
```

```plaintext
2
week09.DivideException: 0으로 나누는 것은 허용되지 않습니다.
	at week09.ExceptionInheritance.divide(ExceptionInheritance.java:18)
	at week09.ExceptionInheritance.main(ExceptionInheritance.java:29)
```
