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

## 프록시 객체?

## 주의점


# 참고
[@Transactional 바르게 알고 사용하기](https://medium.com/gdgsongdo/transactional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%95%8C%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-7b0105eb5ed6)
<br>
[[Spring] @Transactional의 이해](https://imiyoungman.tistory.com/9)
<br>
[[10분 테코톡] 리차드의 @Transactional](https://www.youtube.com/watch?v=taAp_u83MwA)