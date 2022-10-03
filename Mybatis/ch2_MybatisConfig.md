# Mybatis Configuration

### *2021.09.12 Written* 

---

## 0. Mybatis 설정 파일

<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 모든 Mybatis 애플리케이션은 SqlSessionFactory 인스턴스를 사용. JDBC 코드를 Mybatis 코드로 변환하기 위해 가장 먼저 Mybatis 설정 파일을 작성해야 함.
xml 과 자바 설정 파일로 작성할 수 있음<br><br>

### &nbsp;0.1 mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

environment 엘리먼트는 트랜잭션 관리와 커넥션 풀링을 위한 환경을 설정함. mapper 엘리먼트는 SQL 을 정의해둔 xml 이나 인터페이스 형태의 매퍼 위치를 지정해야함. xml 의 경우 classpath: 를 기준으로 지정.<br><br>


