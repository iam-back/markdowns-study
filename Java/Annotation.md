# Annotation

***2022.02.04***

---

## 0.Annotation

### 0.1 Annotation 이란

* 프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것
* annotation 은 주석처럼 프로그래밍 언어에 영향을 미치지 않으면서도 다른 프로그램에게 유용한 정보를 제공할 수 있음

## 1. 표준 어노테이션

JDK 에서 제공하는 어노테이션으로 주로 컴파일을 위한 기능을 제공

**@Override**

* 메소드 앞에만 붙일 수 있는 어노테이션
* 조상의 메소드를 오바라이딩한다는 것을 컴파일러에게 알려줌

**@FunctionalInterface**

* 함수형 인터페이스의 선언부에 붙이며, 컴파일러는 함수형 인터페이스의 제약을 만족했는지를 검사
* 함수형 인터페이스에는 하나의 abstract method 만을 가져야 한다는 제약을 가짐

***메타 어노테이션***

* 어노테이션을 위한 어노테이션
* 어노테이션의 적용대상(target)이나 유지기간(retention)등을 지정하는데 사용

**@Target**

* 어노테이션이 적용가능한 대상을 지정하는데 사용
* METHOD, PARAMETER, TYPE 등의 target 을 가짐

**@Retention**

* 어노테이션의 유지기간을 지정하는데 사용
* SOURCE, CLASS, RUNTIME 의 세가지 유지정책(RetentionPolicy) 가 존재
* 개발자의 경우, 일반적으로 RUNTIME 을 사용하며, 실행 시에 Reflection 을 통해 클래스 파일에 저장된 어노테이션 정보를 읽어 처리함

**@Inherited**

* 어노테이션이 자식 클래스에도 상속되도록 함

**@Repeatable**

* 동일한 어노테이션을 여러 번 붙이고자 할 때 사용

## 2. 어노테이션 Usage

### 2.1 어노테이션 정의

* @ 기호를 붙이는 것을 제외하면 인터페이스를 정의하는 것과 동일
* 아래에서 @Session 을 `어노테이션`, Session 을 `어노테이션 타입` 이라 함 

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.time.LocalDateTime;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Session {
    LocalDateTime validationTime();
}
```

### 2.2 어노테이션 요소

* 어노테이션에 선언된 메소드를 `어노테이션의 요소(element)`라고 한다
* 어노테이션의 요소는 매개변수가 없고 반환값을 가져야 하며, 상속을 통해 구현하지 않아도 됨
* 어노테이션은 상수를 가질 수 있으나, 디폴트 메소드는 정의할 수 없음
* 어노테이션 적용 시, 정의된 모든 요소를 지정해주어야 함
* 어노테이션 요소는 기본값을 가질 수 있음
* 요소 타입이 배열인 경우, 중괄호 {} 를 사용해서 여러 개의 값을 지정할 수 있음
* 요소의 타입은 기본형, String, enum, 어노테이션, Class 만 허용
* 예외를 선언할 수 없다
* 요소를 타입 파라미터로 정의할 수 없다

## 3. java.lang.annotation.Annotation

**모든 어노테이션의 조상 인터페이스**

* 어노테이션은 상속을 허용하지 않으므로 명시적으로 Annotation 을 조상으로 지정할 수 없음


## 4. Marker Annotation

* 값을 지정할 필요가 없는 경우, 어노테이션의 요소를 하나도 정의하지 않을 수 있음
* Serializable 이나 Cloneable 인터페이스처럼, 요소가 하나도 정의되지 않은 어노테이션을 Marker Annotation 이라 함
