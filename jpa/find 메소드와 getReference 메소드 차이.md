# EntityManager 의 find() 와 getReference() 의 차이

---

***결론부터 말하자면, Lazy Loading 을 통한 부하 관리에 있다***

## EntityManager.find(Class<T> var1, Object obj)

* 조회하고자 하는 Entity 를 EntityManager Session 내에서 조회
* 첫번째 파라미터는 Entity 타입을 지정하고 두번째 파라미터는 @Id 로 지정한 식별자의 값을 지정. 매핑되는 Entity 가 없다면, null 반환

```java
UserEntity userEntity = entityManager.find(UserEntity.class, "backee94@gmail.com");
```

## EntityManager.getReference(Class<T> class, Object obj)

* 조회하고자 하는 Entity 의 EntityManager Session 내에서 조회하는 것은 같으나 Proxy 객체를 반환하여 Lazy Initialization 을 지원
* getReference(Class<T> class, Object obj) 는 실행시점에 쿼리를 실행하지 않음
* Session 이 유지된 상태에서 적어도 한 번 Entity 를 조작하여 Proxy 객체가 Entity 의 data 를 Database 에 접근하여 조회해야 함. 만약 Entity 조작없이 Session 을 종료하게 되면 Entity 의 Proxy 객체는 아무런 상태값이 없어 EntityNotFoundException 발생

> ***Lazy Initialization : 지연된 초기화*** <br>
Database 에 대한 접근을 미뤄 Database 에 대한 부하를 의도적으로 늦추기 위한 방법 <br>

```java
/*
 UserEntity 에 대한 Proxy 객체 반환
 아직 아무런 상태값을 가지지 않음
 */
UserEntity userEntity = EntityManager.getReference(UserEntity.class,"backee94@gmail.com");

/// Entity 에 대한 접근이 이루어져야 Database 에 접근하여 Entity 조회 후 상태를 가져옴
String email = userEntity.getEmail();
```

