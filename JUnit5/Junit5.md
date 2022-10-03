# JUnit5 for Java TDD

***2021.09.23 Written***

---

<br>

## 0. JUnit5 란

<br>

>테스트 주도 개발을 위한 Java 진영의 테스팅 프레임워크<br>
> 테스트와 관련된 여러 메소드와 애노테이션을 지원<br>

<br>

* JUnit5 = JUnit Platform + JUnit Jupiter + JUnit Vintage<br><br>
  - JUnit Platform : 테스팅 프레임워크를 구동하기 위한 런처와 테스트 엔진을 위한 API 제공
  - JUnit Jupiter : JUnit5 를 위한 테스트 API 와 실행 엔진을 제공
  - JUnit Vintage : JUnit3 와 JUnit4 로 작성된 테스트를 JUnit5 Platform 에서 실행하기 위한 모듈을 제공

<br>

## 1. @Test 애노테이션과 테스트 메소드

<br>

>테스트를 실행할 메소드는 static 메소드이며 @Test 애노테이션을 붙여 테스트 메소드임을 명시한다.<br>private 접근 제어자로 정의된 메소드는 Test 의 대상이 될 수 없다.

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;


public class CalculatorTest{
    
    @Test
    void sum(){
        int result = 2 + 3;
        assertEquals(5,result);
    }
}
```

<br>

## 2. Assertions class

<br>

>JUnit5 의 Assertions class 에는 값을 검증하기 위한 목적의 다양한 static 메소드를 제공하고 있음

### 2.1 주요 단언 메소드

Assertions class 가 제공하는 대표적인 메소드는 다음과 같다

| 메소드 | 설명 |
|---|---|
| assertEquals(expected, actual) | 실제 값/타입이 기대하는 값/타입과 일치하는지 검사 |
| assertNotEquals(expected, actual) | 실제 값/타입이 기대하는 값/타입과 일치하지 않는지 검사 |
| assertSame(Object expected, Object actual) | 두 객체가 동일한 객체인지 검사 |
| assertNotSame(Object expected, Object actual) | 두 객체가 동일하지 않은 객체인지 검사 |
| assertTrue(boolean condition) | 값이 true 인지 검사 |
| assertFalse(boolean condition) | 값이 false 인지 검사 |
| assertNull(Object actual) | 값이 null 인지 검사 |
| assertNotNull(Object actual) | 값이 null 이 아닌지 검사 |
| fail() | 테스트를 실패처리 |
| assertThrows(Class\<T> expectedType, Executable executable) | executable 을 실행한 결과로 지정한 타입의 예외가 발생하는지 검사 |
| assertDoesNotThrow(Executable executable) | executable 을 실행한 결과로 예외가 발생하지 않는지 검사 |

>***assertXXX 메소드는 실패하면 다음 코드를 실행하지 않고 바로 예외를 발생시킨다.<br>
>따라서 모든 검증을 실행하고 실패한 것만 확인하고 싶다면 assertAll() 의 매개변수로 assertXXX 메소드들을 넘겨주면 가변 인자로 인식하여 처리한 후, 에러 메시지로 보여준다***

<br>

## 3. Test Lifecycle

<br>

> ***JUnit5 는 각 테스트 메소드마다 다음 순서대로 코드를 실행한다***<br>
> 1. 테스트 메소드를 포함한 인스턴스 생성
> 2. @BeforeXXX 애노테이션이 붙은 메소드 실행 (Each 이면 각 테스트 메소드 실행 전, All 이면 모든 테스트 메소드 실행 전 한 번만 수행)
> 3. @Test 애노테이션이 붙은 테스트 메소드 실행
> 4. @AfterXXX 애노테이션이 붙은 메소드 실행 (Each 이면 각 테스트 메소드 실행이 끝난 후, All 이면 모든 테스트 메소드 실행 후 한 번만)

<br>

라이프사이클을 제어하는 애노테이션은 다음과 같다

| 애노테이션 | 설명 |
|---|---|
| @BeforeEach | 인스턴스 생성 후 각 테스트 메소드를 실행하기 전에 실행 |
| @BeforeAll | 인스턴스 생성 후 모든 테스트 메소드를 실행하기 전에 한 번만 실행 |
| @AfterEach | 각 테스트 메소드가 끝날 때마다 실행 |
| @AfterAll | 모든 테스트 메소드의 실행이 끝난 후 한 번만 실행 |

<br>

## 4. 테스트 메소드 간 실행 순서 의존과 필드 공유하지 않기

<br>

> 테스트 메소드가 순서대로 작성되어 있어도 실행 순서는 보장되지 않음<br>
> 테스트 코드 역시 유지보수가 들어가기 때문에 실행 순서 보장과 같은 의존이 생기지 않도록 작성하여야 함<br>
> 따라서 각 테스트 메소드는 독립적으로 실행되도록 하고, static field 를 사용하지 않는 방법으로 의존 관계를 없앨 수 있음

<br>

## 5. @DisplayName, @Disabled 애노테이션

<br>

### 5.1 @DisplayName("name")

특정 테스트 메소드가 실행될 때 콘솔에 표시될 이름을 붙일 수 있음

### 5.2 @Disable 애노테이션

특정 테스트 메소드를 실행하고 싶지 않을 때 붙일 수 있음

<br>

## 6. 모든 테스트 실행하기

> 코드를 원격 리포지토리에 푸시하거나 코드를 빌드해서 운영 환경에 배포하기 전에는 모든 테스트를 실행해서 실패하는 테스트가 없는지 확인해야 함

메이븐과 그레이들의 경우 다음 명령어로 모든 테스트를 실행할 수 있음

* mvn test
* gradle test

<br>

---
***Reference***

최범균, 『테스트 주도 개발 시작하기』가메출판사(2020)