# Transactional

## @Transactional?
- Spring Framework 2.0 이상의 버전에서 지원하는 **@Transactional**은 선언적 데이터베이스 트랜잭션 관리 방법을 제공
- 해당 메서드 또는 클래스의 모든 public 메서드에 트랜잭션을 적용
- 매서드 정상 실행 -> 트랜잭션 commit
- 비정상적 종료(오류) -> 트랜잭션 rollback
=> 트랜잭션 일부 작업만 DB에 반영되는 것을 방지해 데이터 일관성 유지

## 프로그래밍 방식 트랜잭션 관리 VS 선언적 트랜잭션 관리

- 프로그래밍 방식 트랜잭션 관리
``` java
import java.sql.Connection;

Connection conn = dataSource.getConnection();
Savepoint savepoint;

try (connection) {
    conn.setAutoCommit(false);
    Statement stmt = conn.createStatement();
    savepoint = conn.setSavepoint("savepoint");
    String sql = "Insert into User values (16, 'Rosie', 'rosie@gmail.com')"
    stmt.executeUpdate(sql);
    conn.commit(); 
} catch (SQLException e) {
    conn.rollback(savepoint); 
}

```
💥단점
- 코드 실수 가능성 증가
- 중복 코드 증가
- 주된 관심사와 다르게 트랜잭션 관련 코드 증가
- 특정 기술에 종속적인 코드가 됨
    : ex) SQLException은 JDBC에 종속적인 예외 객체 => JPA로 전환 시 모두 변경해야함

<hr>

- 선언적 트랜잭션 관리
``` java
public class UserService{
    private final UserRepository userRepository;   

    @Transactional(readOnly = true)
    public List<User> findAll(User user) {
        return userRepository.save(user);
    }
}
```

## Spring에서의 동작 방식
- 트랜잭션은 Spring AOP를 통해 구현되어있다.

  1) 클래스, 메소드에 @Transactional이 선언되면 해당 클래스에 트랜잭션이 적용된 프록시 객체 생성
  2)  프록시 객체는 @Transactional이 포함된 메서드가 호출될 경우, 트랜잭션을 시작하고 Commit or Rollback을 수행
  3)  CheckedException or 예외가 없을 때는 Commit
  4)  UncheckedException이 발생하면 Rollback


## 주의점

- 트랜잭션을 적용하려는 메서드는 반드시 public으로 선언
- 다른 AOP 기능과의 충돌을 고려
- Service 계층에서 사용
- Exception을 고려
=> RuntimeException과 Error에는 롤백되지만, CheckedExceptions에는 롤백X
- 복잡한 서비스에서 잘못된 트랜잭션으로 인해 DeadLock 발생 가능

<hr>
(아래는 주제로 선정해서 공부 필요할듯) 
<hr>

## Proxy?
- Proxy는 대리자라는 뜻으로, 클라이언트가 사용하려고 하는 실제 대상인 것처럼 위장해서 클라이언트의 요청을 받아주는 역할을 한다. 

<img src ="https://velog.velcdn.com/images/dltkdgus1850/post/139f987c-bfd8-4534-95a2-1bd7c4449e85/image.png">

### 프록시 패턴
- 프록시 객체가 객체를 감싸서 클라이언트의 요청을 처리하게 하는 패턴
- 접근 제어, 부가 기능 추가 등의 이유로 사용
- 원래 객체와 같은 interface를 구현해줘야 하고 객체를 주입받아 interface의 메소드들을 위임받아 사용하고 코드를 추가 해 준다.
- 원래 코드에 손을 쓰지 않고도 기능을 추가 가능
- Spring AOP에서 활용

=> 장점
- OCP: 기존 코드를 변경하지 않고 새로운 기능 추가 가능
- SRP : 기존 코드가 해야하는 일만 유지
- 기능추가, 접근 제어 가능

=> 단점
- 인터페이스, 프록시 객체 등을 구현해야하기에 코드 복잡도 증가
- 모든 코드에 부가기능 구현해야하기에 중복 코드 증가

위와 방식의 단점을 해결하기 위해 
**동적 프록시** 적용

## 동적 프록시

- 인터페이스의 유무에 따라 JDK Dynamic Proxy, CodeGeneratorLibrary(CGLIB)로 나뉨

- JDK Dynamic Proxy
    - JDK에서 지원하는 프록시 생성 방법
    - Invocation Handler를 재정의한 invoke를 구현하여 부가기능 수행
    - Reflection API 사용
    - 인터페이스를 통해서만 프록시 생성 가능

- CGLIB
    - 상속을 기반으로 프록시 생성
    - final이 붙으면 오버라이딩이 불가능하므로 CGLIB 프록시 작동 x







# 참고
[@Transactional 바르게 알고 사용하기](https://medium.com/gdgsongdo/transactional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%95%8C%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-7b0105eb5ed6)
<br>
[[Spring] @Transactional의 이해](https://imiyoungman.tistory.com/9)
<br>
[[10분 테코톡] 리차드의 @Transactional](https://www.youtube.com/watch?v=taAp_u83MwA)
<br>
[Spring-프록시란](https://velog.io/@dltkdgus1850/Spring-%ED%94%84%EB%A1%9D%EC%8B%9C%EB%9E%80)