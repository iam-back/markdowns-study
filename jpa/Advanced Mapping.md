# Advanced Mapping : 고급 매핑

---

## 0. 상속 관계 매핑

* 슈퍼 타입, 서브 타입 논리 모델을 실제 물리 모델인 테이블로 구현할 때는 3가지 방법을 선택할 수 있음
    * 각각의 테이블로 변환 : 조인 전략
    * 통합 테이블로 변환 : 단일 테이블 전략
    * 서브타입 테이블 변환 : 구현 클래스마다 테이블 전략

### 0.1 조인 전략

* 엔티티 각각을 모두 테이블로 만들고 자식 테이블이 부모 테이블의 기본 키를 받아서 기본 키 + 외래 키로 사용하는 전략
* 조회 시 조인을 자주 사용함
* 객체는 타입이 있지만, 테이블은 없으므로 타입 대신 구분할 칼럼이 필요

```java
package jpa.ch7.inheritance;

import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import javax.persistence.*;

@Entity
@Getter
@Builder
@NoArgsConstructor
@Table(name = "ITEM")
@Inheritance(strategy = InheritanceType.JOINED)
@DiscriminatorColumn(name = "DTYPE")
public class ItemEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "ITEM_ID")
    private long id;

    private String price;

    private String dType;
}
```

* @Inheritance
    * 슈퍼 클래스에서 상속 관계 매핑 전략을 선택
* @DiscriminatorColumn
    * 서브 타입을 구분할 칼럼명을 입력

```java
package jpa.ch7.inheritance;

import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import javax.persistence.*;

@Entity
@Getter
@Builder
@NoArgsConstructor
@Table(name = "ALBUM")
@DiscriminatorValue(value = "A")
public class AlbumEntity extends ItemEntity {

    //id,price,dtype 의 경우 ItemEntity 의 값을 상속

    @Column(name = "ARTIST")
    private String artist;

}
```

* extends ItemEntity
    * 슈퍼 타입인 ItemEntity 를 상속받아 공통 필드를 상속받음
* @DiscriminatorValue
    * 서브 타입 구분 시 AlbumEntity 를 구분할 수 있는 값을 입력
    
### 0.2 단일 테이블 전략

* 이름 그대로 테이블을 하나만 사용
* 구분 칼럼은 반드시 필요하며, 자신이 사용하지 않는 속성에 대해서 null 허용해야 함

## 0.3 @MappedSuperClass

* 앞에서의 방법과는 달리 부모 클래스는 테이블과 매핑하지 않고 부모 클래스를 상속 받는 자식 클래스에게 매핑 정보만 제공할 때 사용
* 공통적으로 물려받는 속성에 대해서는 클래스로 구분
* 테이블에서는 각각의 테이블에 구분없이 존재
* 즉, 소스코드 상으로만 분리한 것

