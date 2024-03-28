# JDBC, SQL Mapper, ORM

## persistence(성속성)

- persistence?
  데이터를 생성한 프로그램의 실행이 종료되더라도 사라지지않는 데이터 특성

JDBC, SQL Mapper, ORM은 데이터들이 사라지지 않도록 하는 persistence와 관련이 있다.

프로그램이 실행된 후 객체지향의 데이터들은 객체 생성 후 RAM에 저장되어 있다가 프로그램이 종료되면 날라가게 된다. 온라인을 활용한 비즈니스가 증가하면서 데이터의 영속성은 반드시 필요했고 데이터베이스를 활용해 데이터들이 지워지지 않도록 하였다.

<img src="https://herbertograca.files.wordpress.com/2017/07/2010s-layered-architecture.png">

90년대 이후 layerd architecture를 다음과 같이 나눠 브라우저를 통해 사용자의 요청이 들어와 domain model에서 가공을 통해서 위의 3가지 기술(JDBC, SQLMapper, ORM)이 포함된 persistence layer를 거쳐 데이터에 영속성을 부여했다.

## 1. JDBC

<b> JDBC란? </b>

- Java Database Connectivity 의 약자
- 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API
- 자바 애플리케이션에서 DBMS의 종류에 상관없이, 하나의 JDBC API를 이용해 DB 작업을 처리
- 각각의 DBMS는 이를 구현한 JDBC 드라이버를 제공한다

<b>JDBC의 단점</b>

- 간단한 SQL을 실행하는 데도 중복된 코드를 반복적으로 사용
- Connection과 같은 공유 자원을 제대로 릴리즈(반환) 해주지 않으면 시스템의 자원이 바닥나는 버그 발생
- DB에 따라 일관성 없는 정보를 가진 채로 Cheked Exception (SQLException)처리
  <b>Cheked Exception이란?</b>
  => Checked Exception은 컴파일 단계에서 확인 가능한 예외이다.
  => 다른 말로는 "Compiletime Exception"이라고 한다.
  => Checked Exception은 try/catch로 감싸거나 throw로 던지는 처리를 반드시 해주어야 한다.
  => 예외처리를 컴파일러가 강제하는 것이다.

## 2. SQL Mapper

<b> SQL Mapper란?</b>

- 자바 Persistence Framework 중 하나입니다.
- SQL을 직접 작성합니다.
- SQL 문과 객체(Object)의 필드를 매핑하여 데이터를 객체화

<b>SQL Mapper의 장점</b>

- 쿼리 수행 결과와 객체의 필드를 맵핑 & RowMapper 재활용
- JdbcTemplate가 JDBC에서 반복적으로 해야 하는 많은 작업들을 대신해줌

sql mapper에 속하는 대표적인 프레임워크로 MyBatis가 있다.

### MyBatis

- 반복적인 JDBC 프로그래밍을 단순화합니다.
- SQL 쿼리들을 XML 파일에 작성하여 코드와 SQL을 분리하여 관리합니다.
- JDBC만 사용하면 결과를 가져와서 객체의 인스턴스에 매핑하기 위한 많은 코드가 필요하겠지만,
  마이바티스는 그 코드들을 작성하지 않아도 되게 해 줍니다.
- 동적 쿼리를 작성할 수 있습니다.

<b>동적 쿼리란?</b>
<img src ="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc893sN%2FbtrwmYLw3ch%2FnayqfKKqYXfmkSmJbIsXz1%2Fimg.png">
위는 마이바티스가 데이터를 Access 하는 순서를 나타내는 것

- 스프링 부트에서 구현할 때 보라색 부분인 Mapper Interface와 Mapping File을 구현하게 되면 자연스럽게 객체의 필드와 SQL문이 매핑되게 됩니다.
- Mapper Interface에 대한 구현체를 우리가 구현하지 않아도 마이바티스에서 자동으로 생성하게 됩니다.

<b>마이바티스의 장점</b>

- 자동으로 Connection 관리를 해주면서 JDBC를 사용할 때의 중복 작업 대부분을 없애준다.
- 복잡한 쿼리나 다이나믹하게 변경되는 쿼리 작성이 쉽다.
- 관심사 분리
  => DAO로부터 SQL문을 분리하여 코드의 간결성 및 유지보수성을 향상할 수 있습니다.

## JDBC 와 MyBatis의 단점

- SQL을 직접 다룸으로 문제점이 생기게 된다.
- 특정 DB에 종속적으로 사용하기 쉽습니다.
- 테이블 마다 비슷한 CRUD SQL 작성은 DAO 개발이 매우 반복되는 작업입니다.
- 테이블 필드가 변경될 시 이와 관련된 모든 DAO의 SQL문, 객체의 필드 등을 수정해야 합니다.
- 코드상으로 SQL과 JDBC API를 분리했다 하더라도 논리적으로 강한 의존성을 가지고 있습니다.
- SQL문을 직접 작성하면 SQL에 의존적인 개발을 하게 됩니다.
  데이터 베이스에 항상 의존 관계를 가지게 됩니다.

## ORM

<b>ORM 이란?</b>

- 객체와 관계형 데이터베이스를 매핑하는 것
- 객체간의 관계를 바탕으로 SQL문을 자동으로 생성하고 직관적인 코드(메서드)로 데이터를 조작 할 수 있습니다.
  SELECT \* FROM user; -> user.findAll();
- ORM의 대표적인 예로 JPA가 있습니다.

<b>ORM의 장점</b>

- 패러다임 불일치 문제 해결
  => 객체지향 언어가 가진 장점을 활용할 수 있습니다.

- 생산성
  => 지루하고 반복적인 CRUD 용 SQL을 개발자가 작성하지 않아도 됩니다.

- 데이터 접근 추상화, 벤더 독립성
  => 데이터베이스 벤더마다 미묘하게 다른 데이터 타입, SQL을 손쉽게 해결

- 유지보수
  => 필드 추가, 삭제 시 관련된 CRUD 쿼리를 직접 수정하지 않고, 엔티티를 수정하면 됩니다.

<b>ORM의 단점</b>

- 복잡한 쿼리 사용이 어렵습니다
