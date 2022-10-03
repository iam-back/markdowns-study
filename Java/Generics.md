# Generics

***2022.02.04***

---

## 0. Generics

### 0.1 정의

> * 다양한 타입의 객체들을 다루는 메소드나 컬렉션 클래스에 `컴파일 타임에서 타입 체크를 해주는 기능`
> * 객체의 타입을 컴파일 타임에 체크하기 때문에 객체의 타입 안정성이 높고 형변환의 번거로움이 줄어듦
>
> ***타입 안정성***
>
> 의도하지 않은 타입의 객체가 저장되는 것을 막고, 저장된 객체를 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄여줌

### 0.2 선언 및 특징

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable{
    
    //  생략
    
}
```

* 타입 파라미터 (Type Parameter)
    * 제네릭 클래스로 다루고자 하는 객체 타입 : E
    * 인스턴스 변수로 정의된다
    
* 원시 타입 (Raw Type)
    * 컴파일 시 타입 파라미터가 대입되고 제네릭 타입이 제거된 객체 타입 : ArrayList
    * 위의 같은 경우 타입 파라미터 E 가 대입된 후 컴파일 된 ArrayList 가 원시 타입이 된다
    
* 제네릭 타입 (Generic Type/Generic Class)
    * 다양한 타입 파라미터가 공통적으로 다루어야 하는 명세를 담고 있는 객체 : ArrayList\<E>
    * 형변환의 번거로움을 없애줌

### 0.3 주의

1. 타입 파라미터로 인해 주의해야 할 점
    * 타입 파라미터는 인스턴스 변수이기 때문에 static 멤버에 사용할 수 없다
    * 같은 이유로 new 연산자를 사용한 객체 생성과 instanceof 사용이 불가하다. 컴파일 타임에 객체 타입을 정확히 알아야 하기 때문
    * new 연산자의 사용이 필요하다면, Object 배열을 형변환하거나 Reflection API 를 사용할 수 있다.
    

2. 객체 생성 시 상속 관계로 인해 주의해야 할 점
    * **타입 파라미터가 상속관계**에 있다고 해도 대입될 수 없다. 타입 파라미터는 무조건 같아야 한다.
    * **제네릭 타입이 상속관계**에 있는 경우, 대입된 타입 파라미터가 같다면 가능하다.

```java

import java.util.ArrayList;
import java.util.List;

public class Main {

    public static void main(String[] args) {
        List<Number> list = new ArrayList<>();  //제네릭 타입이 상속관계인 경우는 가능
        list = new ArrayList<Integer>();        //타입 파라미터가 상속 관계예도 불가능
    }
}
```

<br>

## 1. 타입 파라미터의 용법

### 1.1 제한된 제네릭 타입 : extends

* **객체 생성에 있어 특정 타입의 자식들만 대입할 수 있게 타입 파라미터를 제한하려는 경우 extends 키워드를 사용한다**
* 클래스와 인터페이스 상속에 관계없이 extends 를 사용한다
* 제한을 두면서, 인터페이스도 구현해야 한다면 & 로 연결한다
* 아래의 경우 HashMap 의 Key 로 Number 의 자식 타입만 대입 가능하다


```java
public class HashMap<K extends Number & Comparable<Number>,V> extends AbstractMap<K,V>
        implements Map<K,V>, Cloneable, Serializable {
    //생략
        }
```

### 1.2 와일드 카드 : ?

* **메소드 호출에 있어 특정 타입의 자식과 부모들만 대입할 수 있게 타입 파리미터를 제한하려는 경우 와일드 카드를 사용한다**
* 메소드에 다형성을 적용하고자 할 때 사용한다
* 컴파일 시 원시 타입이 된다는 제네릭 타입의 특성상 불가능했던 메소드 오버로딩을 해결해준다
* 타입 파라미터의 상한과 하한을 둘 수 있다

```
<? extends T> : 와일드 카드의 상한 제한. T와 그 자손들만 가능
<? super T> : 와일드카드의 하한 제한. T와 그 부모들만 가능
<?> : <? extends Object> 와 동일
```

<br>

## 2. 제네릭 메소드

* 메소드의 선언부에 타입 파라미터가 선언된 메소드를 제네릭 메소드라 한다
* 메소드 선언부의 타입 파라미터는 제네릭 타입의 타입 파라미터와는 상관없는 파라미터이며, 제네릭 메소드 내에서만 사용되는 지역 변수의 성격을 가진다
* 제네릭 메소드의 타입 파라미터는 리턴 타입 바로 앞에 위치한다
* static 멤버에는 타입 파라미터를 사용할 수 없지만, 메소드에 선언하면 사용 가능하다
* 메소드의 파라미터에 공통적으로 사용할 부분을 분리하는 데 사용할 수 있다

```java
import java.util.Comparator;

class Box<T> {

    static <T> void sort(List<T> list, Comparator<? super T> C){
        // Box 의 타입 매개변수와는 상관없다!
    }
}
```