# Thread (2)

***2021.09.23 Written***

---

<br>

## 0. Thread Group

<br>

* 스레드 그룹은 서로 관련된 스레드를 그룹으로 다루기 위한 것
* 스레드 그룹은 보안상의 이유로 도입된 개념으로, 자신이 속한 스레드 그룹이나 하위 스레드 그룹은 변경할 수 있지만 다른 스레드 그룹의 스레드를 변경할 수 없음
* 스레드를 스레드 그룹에 포함시키려면 Thread 생성자 메소드의 파라미터로 설정해야 함

```java
public class Thread implements Runnable {

    private ThreadGroup group;
    
    public Thread(ThreadGroup group, Runnable target, String name) {
        this(group, target, name, 0);
    }
    
}
```

### 0.1 Thread Group 의 특징

* 모든 스레드는 반드시 스레드 그룹에 포함되어 있어야 함
* 스레드 그룹을 지정하지 않은 스레드는 기본적으로 자신을 생성한 스레드의 스레드 그룹에 속하도록 설정되어 있음
* 자바 애플리케이션이 실행되면, JVM 은 main 과 system 이라는 스레드 그룹을 만들고 JVM 운영에 필요한 스레드들을 생성하여 이 스레드 그룹에 포함
* 우리가 생성하는 모든 스레드 그룹은 main 스레드 그룹의 하위 스레드 그룹에 속함

<br>

## 1. Daemon Thread

<br>

* 데몬 스레드는 다른 일반 스레드의 작업을 돕는 보조적인 역할을 수행하는 스레드
* 일반 스레드가 모두 종료되면 데몬 스레드는 존재 의미가 없기 때문에 강제적으로 자동 종료
* 데몬 스레드는 실행 후 대기하고 있다가 특정 조건이 만족되면 작업을 수행하고 다시 대기상태에 들어감
* 데몬 스레드의 예로는 가비지 컬렉터, 워드프로세서의 자동저장, 화면자동갱신 등이 있음

```java
public class Thread implements Runnable{

    private boolean daemon = false;

    //on 의 값이 true 이면 Daemon Thread 로 설정
    public final void setDaemon(boolean on) {
        checkAccess();
        if (isAlive()) {
            throw new IllegalThreadStateException();
        }
        daemon = on;
    }
}
```

<br>

## 2.Thread 의 실행제어

<br>

* 멀티스레드 프로그래밍은 동기화와 스케줄링을 고려하여야 함
* 스레드의 스케줄링을 위해서는 스레드의 상태와 관련 메소드에 대해 알아야 함

### 2.1 Thread 의 상태

| 상태 | 설명 |
|------|------|
| NEW | 스레드가 생성되고 아직 start() 가 호출되지 않은 상태 |
| RUNNABLE | 실행 중 또는 실행 가능한 상태 |
| BLOCKED |  동기화블럭에 의해서 일시정지된 상태( lock 이 풀릴 떄까지 대기) |
| WAITING, TIMED_WAITING | 스레드의 작업이 종료되지는 않았지만 실행가능하지 않은(unrunnable) 일시정지 상태, TIMED_WAITING 은 일시정지시간이 지정된 경우를 의미 |
| TERMINATED | 스레드의 작업이 종료된 상태 |

