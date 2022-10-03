# Enum

***2022.02.04***

---

## 0. Enum

### 0.1 Enum 이란

> 서로 관련된 상수를 편리하게 선언하기 위해 사용함


예를 들어 사용자 별로 접근 권한을 나누고자 한다.<br>
먼저 Enum 을 사용하지 않고 primitive type 으로 권한을 정해보자.

```java
class User{
    final int ADMINISTRATOR = 0;
    final int MANAGER = 1;
    final int USER = 2;
    
    int authority;
}
```

위와 같은 경우 authority 의 명세를 다시 찾아봐야하는 번거로움이 있다.
이를 Enum 으로 처리해보면,

```java
class User {
    
    private enum Authority {ADMINISTRATOR, MANAGER, USER};

    private Authority authority;
}
```

이와 같이 Enum 으로 처리하게 되면, 시각적으로 정확한 정보를 줄 수 있다는 장점이 있다.

### 0.2 특징

* 타입에 안전한 열거형을 제공함. 즉, 값이 같아도 타입이 다르면 컴파일 에러를 발생
* enum type 의 상수는 primitive type 의 상수와는 달리 상수값이 바뀌어도 다시 컴파일하지 않음

## 1. Enum Usage

### 1.1 선언 및 특징

```java
enum 열거형이름{상수명1, 상수명2, 상수명3}
```

* Enum 에 정의된 상수를 사용하는 방법은 `열거형이름.상수명` 이다
* 모든 열거형은 `java.lang.Enum` 클래스의 자식이다
* 열거형 상수간의 동등 비교는 `==` 로 가능하지만, 다른 비교 연산은 `compareTo()` 를 통해서만 가능하다

### 1.2 멤버 추가하기

* Enum 클래스에 멤버를 추가하기 위해서는 우선적으로 열거형 상수가 먼저 정의되어야 한다
* 열거형의 생성자는 묵시적으로 private 이다. 따라서 생성자 메소드를 호출할 수 없다
```java
public enum Authority {
    
    ADMINISTRATOR, MANAGER, EMPLOYEE, USER;
    
    private final Authority authority;
    
    public Authority getAuthority(){
        return this.authority;
    }
    
}
```

## 2. Enum 에 대한 이해

***Enum 에 정의한 상수 하나하나는 객체다***

위의 Authority 객체는 다음과 같이 쓸 수 있다.

```java
public class Authority{
    static final Authority ADMINISTRATOR = new Authority("ADMINISTRATOR");
    static final Authority MANAGER = new Authority("MANAGER");
    static final Authority EMPLOYEE = new Authority("EMPLOYEE");
    static final Authority USER = new Authority("USER");

    private final Authority authority;

    public Authority getAuthority(){
        return this.authority;
    }
}
```
* `==` 연산이 가능했던 이유는 객체의 주소값을 비교했기 때문이다