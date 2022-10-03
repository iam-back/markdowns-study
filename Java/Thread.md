# Thread

*2021.09.14 Written*

---

## 0. 프로세스와 스레드

<br>

### 0.1 프로세스

* 프로그램을 수행하는 데 필요한 데이터와 메모리 등의 자원 그리고 쓰레드로 구성
* 둘 이상의 스레드를 가진 프로세스를 '멀티스레드 프로세스' 라고 함
* 모든 프로세스에는 적어도 하나의 스레드가 존재

### 0.2 스레드

* 프로세스의 자원을 이용해서 실제로 작업을 수행
* 프로세스가 가질 수 있는 스레드의 갯수는 제한되어 있지 않으나, 스레드가 작업을 수행하는데 개별적인 호출스택을 필요로 하기 때문에 프로세스의 메모리 한계에 따라 생성할 수 있는 스레드의 수가 결정됨
* 하나의 스레드가 예외가 발생하여 종료되더라도 다른 스레드의 실행에는 영향을 미치지 않음

### 0.3 멀티태스킹과 멀티스레딩

* 대부분의 OS 는 멀티태스킹(다중작업)을 지원하기 때문에 여러 개의 프로세스가 동시에 실행될 수 있음
* 멀티스레딩은 하나의 프로세스 내에서 여러 스레드가 동시에 작업을 수행하는 것
* 멀티스레딩의 경우 하나의 서버 프로세스가 여러 개의 스레드를 생성해서 스레드와 사용자의 요청을 일대일로 처리하도록 구현
* 멀티스레드 프로세스는 여러 스레드가 같은 프로세스 내에서 자원을 공유하면서 작업을 하기 때문에 발생할 수 있는 동기화, 교착 상태와 같은 문제들을 고려해서 구현해야 함

<br>

## 1. 스레드의 구현과 실행

<br>

>***스레드는 Thread Class 를 extends 하거나, Runnable Interface 를 implements 하는 방법이 있음***

### 1.1 Thread Class

```java
public class Thread implements Runnable {
    //내부적으로 Runnable Interface 를 구현하고 있음
    
    public Thread(Runnable target) {
        this(null, target, "Thread-" + nextThreadNum(), 0);
        // Thread 로 관리할 Runnable 객체를 넣어준다.
    }
    
}
```

### 1.2 Runnable Interface

```java
public interface Runnable {
    public abstract void run();
    //run() 메소드에 Thread 로 처리할 로직을 구현할 수 있음

}
```

* Runnable 인터페이스를 구현한 경우, Runnable Interface 를 구현한 클래스의 인스턴스를 생성한 다음, 이 인스턴스를 Thread 클래스의 생성자의 매개변수로 제공해야 함
* Runnable 을 구현하면 Thread 클래스의 메소드인 currentThread() 를 호출하여 스레드에 대한 참조를 얻어 와야만 호출이 가능

### 1.3Thread start() Method
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
Thread 의 start() 메소드가 호출되면 실행대기 상태에 있다가 JVM 의 스레드 스케줄러에 의해 자신의 차례가 되면 실행됨
스레드는 한 번 실행이 종료되게 되면 다시 사용할 수 없음 (Reusable 하지 않음). 다시 작업을 수행해야 하면 스레드를 새로운 스레드를 생성하여야 함
하나의 스레드에 대해 start() 메소드가 두 번 이상 호출하면 Runtime 시에 IllegalThreadStateException 이 발생

<br>

## 2. start() & run()

<br>

* start()는 새로운 스레드가 작업을 실행하는데 필요한 call stack 을 생성한 다음에 run()을 호출해서, 생성된 call stack 에 run()이 첫 번째로 올라가게 됨
* 모든 스레드는 독립적인 작업을 수행하기 위해 자신만의 call stack 을 필요로 함
* 새로운 스레드를 생성하고 실행시킬 때마다 새로운 call stack 이 생성되고 스레드가 종료되면 작업에 사용된 call stack 은 소멸

### 2.1 Thread's Call Stack

* 스레드의 호출스택에서는 가장 위에 있는 메소드가 실행중인 메소드이고 나머지 메소드들은 대기상태에 있음
* 스레드가 둘 이상일 때는 call stack 의 최상위에 있는 메소드일지라도 대기상태에 머무름
* JVM 의 스레드 스케줄러는 실행대기중인 스레드들의 우선순위를 고려하여 실행순서와 실행시간을 결정하고 작업을 수행
* 주어진 시간동안 작업을 마치지 못한 스레드는 다시 자신의 차례가 돌아올 때까지 대기상태로 돌아감
* run()의 수행이 종료된 스레드는 call stack 이 모두 비워지며 소멸

### 2.2 Main Thread

main 메소드의 작업을 수행하는 것도 스레드. main 메소드의 스레드가 작업을 마쳤다 하더라도 다른 스레드가 아직 작업을 마치지 않은 상태라면 프로그램은 종료되지 않음.

***프로그램은 실행 중인 스레드가 하나도 없을 때 종료됨***

<br>

## 3. 멀티스레드

<br>

### 3.1 Context Switching

문맥 교환(작업 전환)이라고도 하며, 프로세스 또는 스레드가 교체되는 것을 의미. 현재 진행 중인 작업의 상태 등의 정보를 저장하고 읽어 오는 시간이 소요됨

### 3.2 Concurrent & Parallel

여러 스레드가 여러 작업을 동시에 진행하는 것을 병행(concurrent)이라고 하고, 하나의 작업을 여러 스레드가 나누어 처리하는 것을 병렬(parallel)이라고 함

### 3.3 JVM Thread Scheduler & OS Process Scheduler

JVM 의 Thread Scheduler 에 의해 어떤 스레드가 얼마동안 실행될 것인지가 결정되고, OS 의 Process Scheduler 에 의해 프로세스 실행순서와 실행시간이 결정됨<br>
∴ 매 실행 시 결과가 달라지게 됨

<br>

## 4. 스레드 우선순위

<br>

* 스레드는 우선순위(priority) 라는 상태를 가짐
* 이 우선순위 값에 따라 스레드가 얻는 실행시간이 달라짐
* 작업의 중요도에 따라 스레드의 우선순위를 서로 다르게 지정할 수 있음

### 4.1 Thread priority

```java
public class Thread implements Runnable{

    private int priority;
    public static final int MIN_PRIORITY = 1;
    public static final int NORM_PRIORITY = 5;
    public static final int MAX_PRIORITY = 10;

    public final void setPriority(int newPriority) {
        ThreadGroup g;
        checkAccess();
        if (newPriority > MAX_PRIORITY || newPriority < MIN_PRIORITY) {
            throw new IllegalArgumentException();
        }
        if((g = getThreadGroup()) != null) {
            if (newPriority > g.getMaxPriority()) {
                newPriority = g.getMaxPriority();
            }
            setPriority0(priority = newPriority);
        }
    }
}
```

* 스레드가 가질 수 있는 우선순위는 1~10의 범위를 가지며 숫자가 높을수록 우선순위가 높음. 기본적으로 5로 초기화
* 스레드의 우선순위는 기본적으로 스레드를 생성한 스레드의 우선순위를 상속받도록 설정되어 있음
* JVM Thread Scheduler & OS Process Scheduler 의 방식에 따라 종속적임. 우선순위를 매번 보장하기 어려움

<br>

---

***References***

남궁 성, 『자바의 정석』, 도우출판(2016)
