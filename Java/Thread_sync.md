# Thread for Sync

***2021.09.27 Written***

---

## 0.Synchronization

<br>

* 멀티스레드 환경에서는 각 스레드의 호출 스택 이외의 자원은 공유됨
* 한 스레드가 특정 작업을 끝마치기 전까지 다른 스레드가 공유 자원에 접근하지 않도록 해야 함
* 공유 객체에 접근하는 코드 영역을 임계 구역으로 지정해놓고, 공유 객체가 가지고 있는 lock 을 획득한 단 하나의 스레드만 이 영역 내의 코드를 수행할 수 있도록 함

***이처럼 한 스레드가 진행 중인 작업을 다른 스레드가 간섭하지 못하도록 막는 것을 "동기화 : Synchronization" 라고 함***

### 0.1 Java 에서의 동기화 구현 방법

1. synchronized 키워드
2. java.util.concurrent.locks class
3. java