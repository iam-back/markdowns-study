# Collection Framework for Java

***2021.09.29 Written***

---

## 0. Collection Framework

<br>

> Java API 는 Stack, Queue, List , Map 등의 여러 자료구조를 interface 의 구현체로 지원<br>

### 0.1. Collection interface

* Java Collection Framework(이하 JCF) 는 크게 3가지 타입의 그룹으로 나누어 interface 를 정의하며 List, Set 은 공통 부분을 묶어 Collection interface 로 정의
* Map interface 의 경우 그 성격이 달라 Collection 을 상속받지 못함
* JCF 의 모든 Collection class 들은 List, Set, Map 중 하나를 구현하고 있음. Vector, Hashtable 등의 JCF 이전의 클래스들은 호환성을 위해 존재하며 사용하지 않는 것을 권장 

![Java Collection Framework](https://i2.wp.com/techvidvan.com/tutorials/wp-content/uploads/sites/2/2020/03/collection-framework-hierarchy-in-java.jpg?ssl=1)
출처 : https://techvidvan.com/tutorials/java-collection-framework/

| interface | 특징 |
| --- | --- |
| List \<E> | 순서가 있는 데이터의 집합. 데이터의 중복을 허용 |
| Set \<E> | 순서를 유지하지 않는 데이터의 집합. 데이터의 중복을 허용하지 않음 |
| Map \<K,V> | Key 와 Value 의 쌍으로 이루어진 데이터의 집합. 순서는 유지되지 않으며, Key 는 중복을 허용하지 않고, Value 는 중복을 허용 |

### 0.2 Collection interface methods

대표적인 메소드는 다음과 같다

| 메소드 | 설명 |
| --- | --- |
| boolean add(E e) | 지정된 객체 또는 Collection(c) 의 객체들을 추가 |
| boolean isEmpty() | Collection 이 비어있는지 확인 |
| Iterator\<E> iterator() | Collection 의 iterator 반환 |
| int size() | Collection 에 저장된 객체의 갯수를 반환 |

<br>

## 1. List interface

<br>

> List 인터페이스는 중복을 허용하면서 저장순서가 유지되는 자료구조
> 대표적으로 배열로 구현한 ArrayList 와 노드로 연결한 LinkedList 가 있음


### 1.1 ArrayList

> 내부적으로 Object 배열에 저장하도록 정의되어 있으며, 별다른 index 지정이 없을 경우, 데이터를 순차적으로 저장<br>
> 배열에 더 이상 저장할 공간이 없으면 보다 큰 새로운 배열을 생성해서 기존의 배열에 저장된 내용을 새로운 배열로 복사한 다음에 저장<br>
> index 가 존재해 Read 에 O(1) 시간 복잡도를 보이나 배열 길이가 변경되어야 하는 경우 효율이 떨어지는 단점을 갖는 자료구조

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    private static final int DEFAULT_CAPACITY = 10;
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    transient Object[] elementData; // non-private to simplify nested class access
    
    // Default 생성자의 경우 length 가 10인 비어있는 Object 배열을 생성
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }    
}
```

***ArrayList 주요 메소드***

| 메소드 | 설명 |
| --- | --- |
| boolean add(E e) | ArrayList 의 마지막에 객체를 추가. 성공하면 true |
| int indexOf(Object o) | 지정된 객체가 저장된 위치를 찾아 반환 |
| int lastIndexOf(Object o) | 객체가 저장된 위치르 끝부터 역방향으로 검색해서 반환 |
| Iterator iterator() | ArrayList 의 Iterator 객체를 반환 |
| boolean isEmpty() | ArrayList 가 비어있는지 확인 |
| Object remove(int index) | 지정한 index 의 객체를 반환하고 ArrayList 에서 제거 |
| boolean remove(Object o) | 지정한 객체를 제거 |
| int size() | ArrayList 에 저장된 객체의 갯수를 반환 |
| void sort(Comparator c) | 지정된 정렬기준으로 ArrayList 를 정렬 |
| void trimToSize() | 데이터의 크기에 맞게 배열의 크기를 줄임 |



### 1.2 LinkedList

> 배열로 구현한 ArrayList 와는 달리 연결 리스트는 노드(node)로 구현되어 있으며, 각 노드는 다른 앞뒤의 노드를 참조하도록 구현되어 있는 List<br>
> Read 에 O(n) 시간 복잡도를 가지나 비순차적인 Update 에 강점을 갖는 자료구조

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable {
    
    transient int size = 0;
    //첫번째 노드를 참조하는 Header
    transient Node<E> first;
    //마지막 노드를 참조하는 Tail
    transient Node<E> last;
}
```

***LinkedList 의 주요 메소드***

LinkedList 에는 Queue, Dequeue interface 도 구현하도록 되어있음<br>
따라서 중복되는 기능의 경우 내부적으로 Queue 와 Dequeue 메소드를 호출하도록 정의

| 메소드 | 설명 |
| --- | --- |
| LinkedList() | LinkedList 객체 생성 |
| boolean add(E e) | 지정된 객체를 LinkedList 의 끝에 추가 |
| void add(int index, E e) | 지정된 index 에 객체 추가 |
| E get(int index) | 지정된 index 의 객체 반환 |
| E element() | 첫 번째 요소를 반환(getFirst() 를 호출) |
| E remove() | 첫 번쨰 요소를 반환한 뒤 제거(removeFirst() 를 호출) |

### 1.3 Stack & Queue

* Stack : 후입선출
    * 마지막에 저장한 데이터를 가장 먼저 반환하게 되는 자료구조
    * 순차적으로 저장하고 접근하기 때문에 ArrayList 로 구현
    
* Queue : 선입선출
    * 먼저 저장된 데이터가 가장 먼저 반환하게 되는 자료구조
    * 데이터의 변경이 잦으므로 LinkedList 로 구현
    
<br>

## 2. Iterator interface

<br>

> Iterator 는 컬렉션에 저장된 요소를 접근하는데 사용되는 표준화된 interface

```java
public interface Iterator<E> {
  boolean hasNext();
  E next();
  default void remove() {
    throw new UnsupportedOperationException("remove");
  }
}
```

***Iterator 주요 메소드***

| 메소드 | 설명 |
| --- | --- |
| boolean hasNext() | 읽어 올 요소가 남아있는지 확인 |
| Object next() | 다음 요소를 읽어 옴 |
| void remove() | next() 로 읽어 온 요소를 삭제 |

* Map interface 를 구현한 클래스의 경우, key 와 value 의 쌍으로 이루어진 구조이기 때문에 iterator() 를 직접 호출할 수 없음
* 그 대신 keySet() 이나 entrySet() 과 같은 메소드를 통해 키와 값을 각각 따로 Set 의 형태로 가져온 후 iterator() 를 호출하여 얻을 수 있음
* 선택적으로 구현할 abstract 메소드의 경우 remove() 와 같이 예외를 발생시키도록해서 나중으로 구현을 미룰 수 있음


