# Singleton Pattern 과 그 목적

---

> ***결론부터 말하자면*** <br>
Singleton Pattern 은 단 하나의 instance 화된 object 를 통해 application 을 설정하기 위함이다.

## Objectives

0. Singleton Pattern 에 대해 알아본다
1. Java application 에서 instance 를 생성할 떄, Runtime 시 Memory 에서 어떤 일이 일어나는지 알아본다
2. Application 에서의 Singleton Pattern 의 쓰임과 의의
3. Spring 에서 어떻게 DI 를 구현하는지 알아본다

## 0. Singleton Pattern

***객체에 대한 인스턴스를 하나만 만들어 공유하는 패턴***

Java 에서는 객체에 대한 instance 를 new 연산자를 통해 메모리에 할당할 수 있다.

```java
public class Configuration{

    public Configuration(){
    }

    public static void main(String[] args){

        //생성자를 통해 instance 를 생성
        Configuration config = new Configuration();
    }
}

```
Singleton Pattern 은 생성자를 외부에서 사용할 수 없게 하는 대신 인스턴스를 생성하여 반환하는 method 를 통해 제공함으로써, 객체의 동일성을 보장할 수 있다.
```java
public class Configuration{

    //instance 를 singleton 으로 할당할 static 변수
    private static Configuration instance;
    

    //constructor 를 외부로부터 접근할 수 없도록 함
    private Configuration(){
    }

    /*
    method 를 통해서만 instance 에 접근할 수 있도록 함
    instance 를 생성하지 않고도 호출할 수 있어야 하므로 static 으로 정의
    */
    public static Configuration getInstance(){

        //instance 를 기존에 생성했으면 해당 instance 를 반환
        if (this.instance!=null){
            return this.instance;
        }

        return this.instance = new Configuration();
    }

    public static void main(String[] args){
        
        //method 를 통해 접근해야 한다.
        Configuration config = Configuration.getInstance();
    }
}

```

***Notice*** <br>
위의 예시에서 Configuration 이라는 이름으로 class 를 작성한 것에 주의하며 보기 바란다. 후술하겠지만, Application 관점에서 Singleton 으로 관리하는 객체의 특성을 보여준다.


## 1. Runtime Data Areas 그리고 GC

### Runtime Data Areas
Java Source code(.java) 는 Java Compiler 에 의해 byte code(.class) 로 변환되고 이를 JVM 을 통해 실행하게 된다. <br>
JVM 은 Java Application 을 실행시키기 위한 프로그램으로 컴파일된 class 파일을 class loader 를 통해 메모리에 적재한다. 이 때, class 에 대한 정보는 method 영역에, 런타임에 생성되는 instance 는 heap 영역에 올라가며, 이는 모든 thread 가 공통으로 접근할 수 있는 영역으로 하나만 존재한다.

### Gabage Collection 과 Stop-the-world

Java 가 C/C++ 과 다른 점은 메모리에 대한 관리를 개발자가 직접하지 않는다는 점에 있다. 개발자는 할당만 할 수 있을 뿐, 해제에 대한 권한은 전적으로 JVM 의 GC(Gabage Collector) 에게 주어진다. GC 는 더 이상 참조하지 않는 instance 를 해제함으로써 메모리를 관리하는데, 이 때 Application 은 GC 에 의해 잠시 멈추게 되고, 이를 Stop-the-world(STW) 라 부른다.

## 2. Application 에서의 Singleton

다시 말하지만, instance 는 모든 thread 에서 공유가능하다. <br> 여기서 공유가능하다는 것은 `instance 가 단 한번 초기화되고 변경되어서는 안됨을 의미`한다. 

### 변경되지 않으며 공유되는 객체를 Signleton 으로 관리하라

앞서 예시에서 Configuration 을 예로 든 것이 바로 그 이유다. 변경되지 않는 설정이 매 thread 마다 생성되는 것은 잦은 STW 의 원인이 될 수 있다. 이러한 객체는 한 번 만들어 공유하는 것이 좋다. <br>
Person 이나 User 와 같은 thread 에 따라 변경되는 DTO/Entity 역할을 수행하는 객체는 Singleton 으로 사용하지 않는다. 단 한번 초기화시켜 서로 공유할 수 있는 객체만을 Singleton 으로 관리한다.<br>

## 3. Spring Bean

Java 에서는 Singleton Pattern 을 통해 관리할 객체를 Bean 이라는 특별한 이름으로 부른다. Spring 에서 제공하는 어노테이션인 @Component, @Service, @Bean, @Repository, @Configuration 은 DI 의 대상으로 정의하게 되며, 이는 Spring Container 의 관리대상이 된다.

```java
package config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@Import(value = {PersistenceConfig.class, PresentationConfig.class})
@ComponentScan(basePackages = {"controller", "service.core.impl","service.util.impl", "data.repository"})
public class ApplicationConfig {
}

```