---
title: "[Live Study] 12주차 과제 : 어노테이션"
date: 2021-02-05 22:00:00 +0900
categories: [Java, Live Study]
tags: [java, anotation]
---

[![study-halle](/assets/img/posts/java/livestudy/study-halle.png)](https://github.com/whiteship/live-study)

이 스터디는 백기선님과 시청자분들이 함께하는 온라인 자바 스터디입니다.

- [https://www.youtube.com/channel/UCwjaZf1WggZdbczi36bWlBA](https://www.youtube.com/channel/UCwjaZf1WggZdbczi36bWlBA)
- [https://github.com/whiteship/live-study](https://github.com/whiteship/live-study)

<br/>

## 어노테이션이란?

어노테이션이란 일종의 메타데이터라고 볼 수 있다.

메타데이터란 데이터에 대한 속성 정보이다. 데이터에 대한 데이터로서 하위 레벨 데이터를 설명 및 기능하는데 사용되는 데이터라고 할 수 있다.

즉, 어플리케이션의 컴파일 과정, 실행 과정에서 코드를 어떻게 컴파일하고 처리할 것인지를 알려주는 정보이다.

- 어노테이션의 용도
  - 코드 문법 에러 체크
  - 코드 자동 생성 정보 제공
  - 런타임 시 특정 기능을 실행하는 정보 제공

## 어노테이션 정의하는 방법

```java
public @interface MyAnnotation {
    ...
}
```

`@interface`는 어노테이션 타입(annotation type)을 선언하는 키워드로 어노테이션 타입 선언을 일반적인 인터페이스(interface) 선언과 구분하기 위해 interface 앞에 기호 @를 붙인다.

### 어노테이션의 요소

어노테이션은 필드같은 요소들을 정의하는 것이 가능하며 요소의 규칙은 다음과 같다.

- 요소의 타입은 기본형, String, enum, 어노테이션, Class만 허용된다.
- ()안에 매개변수는 선언할 수 없다.
- 예외를 선언할 수는 없다.
- 요소를 타입 매개변수로 정의 할 수 없다.

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyAnnotation {
    int number() default 500;   //int 기본형
    String value();             //String 문자열
    String[] arr();             //Array 배열
    Operation operation();      //Enum 타입
    Class clazz();              //Class 타입
    Target target();            //Target 어노테이션
}
```

## 표준 어노테이션

### @Orverride

오버라이드를 할 때 사용되며, 메소드가 오버라이드 되었는지 알려주는 역할을 한다. 오버라이드는 생략이 가능하지만 미연의 실수를 방지하기 위해서 생략하지 않는 것이 바람직하다.

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```

### @Deprecated

앞으로 사용되지 않을 대상에 붙여지는 어노테이션으로 자바버전이 올라감에 따라 더 이상 사용을 권장하지 않는 것들에 대해 표시할 때 사용된다.

가장 대표적인 예로 Date 클래스의 생성자를 살펴보면 @Deprecated 어노테이션이 존재하는 것을 확인할 수 있는데, 해당 클래스를 삭제하면 기존에 사용하고 있는 프로그램이 동작하지 않기 때문에, 기존의 사용은 유지하되 앞으로의 사용으 권장하지 않는다는 의미이다.

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
public @interface Deprecated {
    String since() default "";
    boolean forRemoval() default false;
}
```

### @FunctionalInterface

함수형 인터페이스를 의미한다. 함수형 인터페이스는 이후 람다에 대해 학습할 때 나오겠지만, 해당 인터페이스는 단 하나의 추상 메소드만을 가질 수 있다는 규칙을 가지고 있다.

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface FunctionalInterface {}
```

### @SupperessWarnings

컴파일러가 보여주는 경고 메시지가 보이지 않게 억제해주는 어노테이션

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
    String[] value();
}
```

## 메타 어노테이션

- 어노테이션을 위한 어노테이션
- 어노테이션을 정의할 때 어노테이션의 적용대상, 유지시간 등을 지정하는데 사용

### @Retention

어노테이션의 유지 정책을 지정하는 어노테이션

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Retention {
    /**
     * Returns the retention policy.
     * @return the retention policy
     */
    RetentionPolicy value();
}
```

- 어노테이션 유지 정책

  - SOURCE
    - 소스 파일에만 어노테이션 정보를 유지
    - 컴파일 이후 어노테이션 정보가 유지되지 않음
  - CLASS
    - 컴파일 이후 클래스 파일까지 어노테이션 정보를 유지
    - 기본 값이지만 런타임시 유지되지 않기 때문에 잘 사용되지 않음
  - RUNTIME
    - 런타임시에도 어노테이션 정보를 유지
    - 실행 시 리플렉션을 통해 클래스 파일에 저장된 어노테이션의 정보를 읽어서 처리 가능

### @Target

적용가능한 대상을 지정하는데 사용되는 어노테이션

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Target {
    /**
     * Returns an array of the kinds of elements an annotation type
     * can be applied to.
     * @return an array of the kinds of elements an annotation type
     * can be applied to
     */
    ElementType[] value();
}
```

- Target의 종류

  | 대상 타입 | 적용 대상 |
  |ANNOTATION_TYPE| 어노테이션|
  |CONSTRUCTOR| 생성자|
  |FIELD| 필드(멤버변수, enum 상수)|
  |LOCAL_VARIABLE| 지역변수|
  |METHOD |메서드|
  |PACKAGE| 패키지|
  |PARAMETER| 매개변수|
  |TYPE| 타입(클래스, 인터페페이스, enum)|
  |TYPE_PARAMETER| 타입 매개변수(JDK1.8)|
  |TYPE_USE| 타입이 사용되는 모든 곳(JDK1.8)|

### @Documented

어노테이션에 대한 정보가 javadoc으로 작성한 문서에 포함되도록 하는 어노테이션으로 자바에서 제공하는 기본 어노테이션 중 @Override, @SuppressWarnings를 제외하고는
모두 이 메타 어노테이션을 사용한다.

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Documented {
}
```

## 어노테이션 프로세서

어노테이션 프로세서는 소스코드 레벨에 붙어있는 어노테이션 정보를 읽어와 컴파일러가 컴파일 중에 새로운 소스코드를 생성하거나 기존 소스코드를 바꿀 수 있다.

또는, 바이트코드도 생성이 가능하며, 별개의 리소스파일을 생성하는 것도 가능하다.

- 어노테이션 프로세서의 사용 예

  - Lombok (기존 코드를 변경)
  - AutoService
    - java.util.ServiceLoader용 파일 생성 유틸리티
  - @Override

- 어노테이션 프로세서의 장점

  - 바이트코드에 대한 조작은 런타임시 발생되기 때문에 런타임에 대한 비용이 발생한다.
  - 하지만, 어노테이션 프로세서의 경우 컴파일 시점에 조작하기 때문에 런타임시 비용이 없다.

- 애노테이션 프로세서의 단점

  - 기존의 코드를 고치는 방법은 현재 public한 API가 존재하지 않는다.
