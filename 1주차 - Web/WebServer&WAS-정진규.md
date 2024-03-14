## Web Server와 WAS의 차이

<br>

<img src="https://gmlwjd9405.github.io/images/web/webserver-vs-was1.png">

<br>

## 웹 서버

<br>

개념에 있어서 하드웨어와 소프트웨어로 구분된다.

**하드웨어** : Web 서버가 설치되어 있는 컴퓨터

**소프트웨어** : 웹 브라우저 클라이언트로부터 HTTP 요청을 받고, 정적인 컨텐츠(html, css 등)를 제공하는 컴퓨터 프로그램

<br>

### 웹 서버 기능

> Http 프로토콜을 기반으로, 클라이언트의 요청을 서비스하는 기능을 담당

요청에 맞게 두가지 기능 중 선택해서 제공해야 한다.

- [정적 컨텐츠](#정적-페이지와-동적-페이지) 제공

  > WAS를 거치지 않고 바로 자원 제공

- [동적 컨텐츠](#정적-페이지와-동적-페이지) 제공을 위한 요청 전달

  > 클라이언트 요청을 WAS에 보내고, WAS에서 처리한 결과를 클라이언트에게 전달

<br>

**웹 서버 종류** : Apache, Nginx, IIS 등

<br>

## WAS

Web Application Server의 약자

> DB 조회 및 다양한 로직 처리 요구시 **동적인 컨텐츠를 제공**하기 위해 만들어진 애플리케이션 서버

HTTP를 통해 애플리케이션을 수행해주는 미들웨어다.

**WAS는 웹 컨테이너 혹은 서블릿 컨테이너**라고도 불림

(컨테이너란 JSP, Servlet을 실행시킬 수 있는 소프트웨어. 즉, WAS는 JSP, Servlet 구동 환경을 제공해줌)

<br>

### 역할

WAS = 웹 서버 + 웹 컨테이너

웹 서버의 기능들을 구조적으로 분리하여 처리하는 역할

> 보안, 스레드 처리, 분산 트랜잭션 등 분산 환경에서 사용됨 ( 주로 DB 서버와 함께 사용 )

<br>

### WAS 주요 기능

1.프로그램 실행 환경 및 DB 접속 기능 제공

2.여러 트랜잭션 관리 기능

3.업무 처리하는 비즈니스 로직 수행

<br>

**WAS 종류** : Tomcat, JBoss 등

<br>

<br>

## 그럼, 둘을 구분하는 이유는?

<br>

-> **목적이 다르다**

```
웹 서버는 정적인 데이터를 처리하는 서버이다. 이미지나 단순 html파일과 같은 리소스를 제공하는 웹 서버를 통하면 빠르고 안정적이다.

WAS는 동적인 데이터를 처리하는 서버이다. DB와 연결되어 데이터를 주고 받거나 프로그램으로 데이터 조작이 필요한 경우에 WAS를 사용한다.

우리가 만드는 웹 페이지는 정적 컨텐츠와 동적 컨텐츠를 함께 노출하게 한다. WAS가 정적 데이터를 처리하게 되면, 동적 컨텐츠의 처리가 지연이 될 것이고 이로 인한 페이지 노출시간이 늘어나게 된다. WAS는 동적 처리에 최적화 되어 있는 서비스이기 때문에 처리 속도를 위해, 정적처리는 웹서버에서 처리를 하고, 동적 컨텐츠는 WAS에서 처리하게 된다.
```

<br>

### WAS로 웹 서버 역할까지 다 처리할 수 있는거 아닌가?

```
WAS는 DB 조회, 다양한 로직을 처리하는 데 집중해야 함. 따라서 단순한 정적 컨텐츠는 웹 서버에게 맡기며 기능을 분리시켜 서버 부하를 방지하는 것

만약 WAS가 정적 컨텐츠 요청까지 처리하면, 부하가 커지고 동적 컨텐츠 처리가 지연되면서 수행 속도가 느려짐 → 페이지 노출 시간 늘어나는 문제 발생
```

<br>

## Web Server와 WAS의 구성에 따른 분류

<br>
 
**■ WAS와 WebServer를 분리하지 않는 경우**

모든 컨텐츠를 한곳에 집중시켜 웹서버와 WAS의 역할을 동시에 수행,

스위치를 통한 로드 밸러싱, 사용자가 적을 경우 효율적

<br>

**■ WAS와 WebServer를 분리한 경우**

웹서버와 WAS의 기능적 분류를 통해 효과적인 분산을 유도,

정적인 데이터는 웹서버에서 처리, 동적인 데이터는 WAS가 처리

 <br>

**■ WAS 여러개와 WebServer를 분리한 경우**

WAS단을 프리젠테이션 로직와 비즈니스 로직으로 구분하여 구성,

특정 logic의 부하에 따라 적절한 대응할 수 있지만 설계단계 유지보수 단계가 복잡해 질 수가 있다.

<br>

**■ WAS와 WebServer를 분리하는 이유**

1. 기능을 분리하여 서버의 부하방지

2. 물리적으로 분리하여 보안강화([Reverse Proxy](https://aday7.tistory.com/entry/%EB%A6%AC%EB%B2%84%EC%8A%A4-%ED%94%84%EB%A1%9D%EC%8B%9CReverse-Proxy-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%ED%95%84%EC%9A%94%EC%84%B1-%EC%98%A4%ED%94%88-%EC%86%8C%EC%8A%A4-%EC%86%94%EB%A3%A8%EC%85%98%EA%B9%8C%EC%A7%80) 역할 수행)

3. 여러대의 WAS를 연결 가능 ([로드밸런싱](https://www.smileshark.kr/post/what-is-a-load-balancer-a-comprehensive-guide-to-aws-load-balancer#viewer-21pmp)의 역할 및 fail over, fail back 처리에 유리)

4. 여러 웹어플리케이션을 서비스 가능(java서버, c#서버, php서버 등 하나의 웹서비스를 통해 서비스 가능)

<br>

### ■ 가장 효율적인 방법

> 웹 서버를 WAS 앞에 두고, 필요한 WAS들을 웹 서버에 플러그인 형태로 설정하면 효율적인 분산 처리가 가능함

<br>

<img src="https://gmlwjd9405.github.io/images/web/web-service-architecture.png">

<br>

## 동작 과정

<br>

클라이언트의 요청을 먼저 웹 서버가 받은 다음, WAS에게 보내 관련된 Servlet을 메모리에 올림

WAS는 web.xml을 참조해 해당 Servlet에 대한 스레드를 생성 (스레드 풀 이용)

이때 HttpServletRequest와 HttpServletResponse 객체를 생성해 Servlet에게 전달

> 스레드는 Servlet의 service() 메소드를 호출
>
> service() 메소드는 요청에 맞게 doGet()이나 doPost() 메소드를 호출

doGet()이나 doPost() 메소드는 인자에 맞게 생성된 적절한 동적 페이지를 Response 객체에 담아 WAS에 전달

WAS는 Response 객체를 HttpResponse 형태로 바꿔 웹 서버로 전달

생성된 스레드 종료하고, HttpServletRequest와 HttpServletResponse 객체 제거

<br>

<br>

<br>

## 정적 페이지와 동적 페이지

<img src="https://gmlwjd9405.github.io/images/web/static-vs-dynamic.png">

### Static Pages

> 바뀌지 않는 페이지

웹 서버는 파일 경로 이름을 받고, 경로와 일치하는 file contents를 반환함

항상 동일한 페이지를 반환함

```
image, html, css, javascript 파일과 같이 컴퓨터에 저장된 파일들
```

<br>

### Dynamic Pages

> 인자에 따라 바뀌는 페이지

인자의 내용에 맞게 동적인 contents를 반환함

웹 서버에 의해 실행되는 프로그램을 통해 만들어진 결과물임
(Servlet : was 위에서 돌아가는 자바 프로그램)

개발자는 Servlet에 doGet() 메소드를 구현함

<br>



## 참고

[WEB과 WAS 차이](https://helloworld-88.tistory.com/71)
<br>
[웹서버와 WAS](https://story.pxd.co.kr/1647)
<br>
[[Web] Web Server와 WAS의 차이와 웹 서비스 구조](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)