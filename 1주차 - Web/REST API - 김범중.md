## 목차

1. [Rest API?](#rest-api)
   <br>
   a. [Rest의 구성](#rest의-구성)
   <br>
   <br>
2. [Rest를 구성하는 6가지 스타일](#rest를-구성하는-6가지-스타일---uniform-interface가-가장-중요해보임)
   <br>
   a. [client-server](#1-client-server)
   <br>
   b. [stateless](#2-stateless무상태)
   <br>
   c. [cache](#3-cache)
   <br>
   d. [layered system](#4-layered-system계층)
   <br>
   e. [code-on-demand(optional)](#5-code-on-demandoptional)
   <br>
   f. **[Uniform Interface](#6--uniform-interface-)**
   <br>
    - [Identification of resources (자원에 대한 식별)](#identification-of-resources-자원에-대한-식별)
    - [Manipulation of resources through representations (표현을 통해 자원을 조작)](#manipulation-of-resources-through-representations-표현을-통해-자원을-조작)
    - [**Self-descriptive message (자기 서술적 메시지: 메시지는 스스로를 설명해야한다.)**](#self-descriptive-message-자기-서술적-메시지-메시지는-스스로를-설명해야-한다)
    - [**HATEOAS(애플리케이션의 상태는 Hyperlink를 이용해 전이되어야 한다.)**](#hateoas애플리케이션의-상태는-hyperlink를-이용해-전이되어야-한다)
      <br>
      <br>

3. [Uniform Interface를 사용하는 이유](#uniform-interface-사용-이유)
4. [참고](#참고)

<br>

## Rest API?

- `Representational State Transfer`의 약자.
- `Rest` 아키텍처를 따르는 API
    - `Rest` : 분산 하이퍼미디어 시스템(ex.웹)을 위한 **아키텍처 스타일**
    - `아키텍처 스타일` : **제약 조건**의 집합

즉, **Rest에서 정의한 제약 조건을 만족하는 API**

<br>

### Rest의 구성

- 자원(Resource) - URL
- 행위(Verb) - Http Method
- 표현(Representations)

<br>

1. **자원 (Resource) URL**

> - 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.<br>
> - 자원을 구별하는 ID는 `/orders/order_id/1` 와 같은 HTTP URI 이다.

  <br>

2. **행위 (Verb) - Http Method**

- HTTP 프로토콜의 GET, POST, PUT, DELETE와 같은 메서드를 제공
  <br>

```text
멱등성(`Idempotent`)에 대해 얘기해보고자 한다.
-> 멱등성이란, 여러번 수행했을 때 결과가 같은 성질을 말한다.

GET: 서버에 존재하는 리소스를 단순히 읽어오는 메소드이기 때문에 `Idempotent`하다.
PUT: 서버에 존재하는 리소스를 요청(Request)에 담긴 내용으로 대체하기 때문에 `Idempotent`하다.
DELETE: 존재하는 데이터를 삭제한 결과(200 OK), 존재하지 않는 데이터를 삭제하려는 시도(404 NOT FOUND)의 STATUS는 다르지만
        항상 리소스가 없는 동일한 상태이기 때문에 `Idempotent`하다.
        
POST: 새로운 리소스가 생성되거나, 리소스의 상태가 달라지면서 호출 결과가 달라질 수 있기 때문에 `Non-Idempotent`하다.
PATCH: 기존 리소스에 응답을 추가(append)하는 경우에도 PATCH가 사용될 수 있어 호출 결과가 달라질 수 있기 때문에 `Non-Idmpotent`하다.
```

<br>

3. **표현 (Representaion of Resource)**

> - Client가 자원의 상태 (정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답 (Representation)을 보낸다.
> - REST에서 하나의 자원은 Json, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타낼 수 있다.
> - 현재는 대부분 `Json`으로 주고 받는다.

<br>

## Rest를 구성하는 6가지 스타일 - [Uniform Interface가 가장 중요해보임]

### 1. client-server

> - 자원(Resource)이 있는 쪽이 Server, 요청하는 쪽이 Client

- Rest Server : API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.

> - Client : 사용자 인증, context(세션, 로그인 정보)등을 관리하고 책임 진다.

-> 서버, 클라이언트의 의존성이 줄어든다.

<br>

### 2. stateless(무상태)

**HTTP**

> - 클라이언트와 서버 사이에 이루어지는 요청/응답(request/response) 프로토콜
> - HTTP 는 비연결성(Connectionless)과 무상태(Stateless)라는 특성을 가진다.

**비연결성(Connectionless)**

> - 클라이언트가 요청(request)을 하고, 서버가 해당 요청에 적합한 응답(response)를 하게 되면 바로 연결을 끊는 성질

**무상태(Stateless)**

> - 비연결적인 특성.
> - 연결이 해제되면 서버와 클라이언트는 클라이언트가 이전에 요청한 결과를 잊어버린다.

- 클라이언트가 이전 요청과 같은 데이터를 원해도, 다시 서버에 연결해 동일한 요청을 시도해야한다.

=> **왜 Connectionless, Stateless??**

- 상태를 서버에 저장하지 않기 때문에 부담을 감소시킬 수 있다.

<br>

**왜 Rest API는 Stateless해야하지?**
<br>
-> 확장 가능성 때문. 서버에 클라이언트의 상태를 저장하지 않기 때문에 `scale-out` 용이함.

<br>

### 3. cache

HTTP 프로토콜을 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있음.
<br>
-> **캐싱 적용 가능**(리소스의 복사본을 저장하고 있다가 요청시에 제공하는 기술)

> - 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구됨.<br>
> - 캐시 사용을 통해 응답시간이 빨리지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있음

<br>

### 4. layered system(계층)

> - REST 서버는 다중 계층으로 구성될 수 있음.
> - (보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다)

<br>

### 5. code-on-demand(optional)

> - Server로부터 스크립트를 받아서 Client에서 실행.

<br>

## 6. ★ uniform interface ★

### Identification of resources (자원에 대한 식별)

자원 : 이름을 지닐 수 있는 모든 정보. (ex.문서, 이미지, 자원들의 집합...)

**자원은 객체이기 때문에 상태가 변할 수 있다.** -> 변하지 않는 `식별자` 필요
<br>
=> **`URI`를 통해 자원을 식별**

<br>

### Manipulation of resources through representations (표현을 통해 자원을 조작)

표현 : 특정한 상태의 `자원`에 대해 `표현`
<br>
![image](https://github.com/2024-SSAFY-CS-Study/2024-SSAFY-CS-Study/assets/121084350/5a664479-c620-4c99-b777-911c52c92607)

![image](https://github.com/2024-SSAFY-CS-Study/2024-SSAFY-CS-Study/assets/121084350/145c6fd8-c306-40f8-b98e-d827398ecebc)


<br>

### Self-descriptive message (자기 서술적 메시지: 메시지는 스스로를 설명해야 한다.)

```
GET / HTTP/1.1
```

루트를 얻어오는 GET 요청. `Self-descriptive`하지 못함.

<br>
<br>

```
GET / HTTP/1.1
Host: www.example.org
```

이렇게 특정 `도메인`으로 가겠다는 `목적지` 정보를 추가하면 `Self-descriptive`하다.

---

```
HTTP/1.1 200 OK
[
  {
    "op": "remove",
    "path": "/a/b/c"
  }
]
```

200 응답 메시지이며, JSON 본문이 있는데 이는 `Self-descriptive`하지 않다.
-> 어떤 문법으로 작성된 것인지 모르기 때문이다.

<br>
<br>

```
HTTP/1.1 200 OK
Content-Type: application/json

[{"op": "remove", "path": "/a/b/c"}]
```

이와 같이 `Content-Type` 헤더를 추가해야한다.
하지만 이도 `Self-descriptive`하지 않다.
op, path가 무엇을 의미하는지 알 방도가 없다.

<br>
<br>

```
HTTP/1.1 200 OK
Content-Type: application/json-patch+json

[{"op": "remove", "path": "/a/b/c"}]
```

`Content-Type` 헤더와 더불어 `Media Type`을 정의해주어야 한다.
이제 json-patch 명세를 찾아보고 해석하면 된다.

<br>

> **※ `Self-descriptive message`는 메시지를 봤을 때 메시지의 내용만으로
온전히 해석이 가능해야 된다는 의미이다.**

---

<br>

### HATEOAS(애플리케이션의 상태는 Hyperlink를 이용해 전이되어야 한다.)

![image](https://github.com/2024-SSAFY-CS-Study/2024-SSAFY-CS-Study/assets/121084350/988e8ae1-aa14-4cf1-b7ca-86448c8cdab0)

이렇게 상태를 전이하는 것을 애플리케이션 상태 전이라고 한다.
<br>
**상태 전이마다 항상 해당 페이지에 있던 링크를 따라가면서 전이했기 때문에 HATEOAS이다.**
<br>

<br>

```
HTTP/1.1 200 OK
Content-Type: text/html

<html>
<head></head>
<body><a href="/test"> test </a></body>
</html>
```

`HTML`의 경우 `HATEOAS`를 만족하게 된다.<br>
a 태그를 통해 하이퍼링크가 나와있고, 이를 통해 다음 상태로 전이가 가능하기 때문이다.

<br>
<br>

```
HTTP/1.1 200 OK
Content-Type: application/json
Link: </articles/1>; rel="previous",
</articles/3>; rel="next";

{
"title": "The second article",
"contents": "blah blah..."
}
```

`Json`의 경우 **Link라는 헤더가 있는데, 이 리소스와 하이퍼링크로 연결되어있는 다른 리소스를 가리키는 기능**을 이용하면 된다.
<br>
<br>
위 `Json`의 경우 이전의 게시물 URI가 `/articles/1`, 다음 게시물은 `/articlees/3`에 있다는 것을 나타낸다.

<br>

## Uniform Interface 사용 이유

### 독립적 진화

> - 서버와 클라이언트가 각각 독립적으로 진화할 수 있다.
> - **서버의 기능이 변경되어도 클라이언트를 업데이트 하지 않아도 된다.**

```text
Roy Fielding이 초반에 http 1.0을 만들 당시 고민했던
"웹을 망가뜨리지 않고 어떻게 수정할 것인가?"에 대한 결과가 REST라고 한다.

그렇기 때문에 REST가 목적하는 바가 독립적인 진화이다.
이를 달성하기 위해서는 Unifrom Interface가 필수적이기기에, 이를 만족하지 못하면 REST라고 부를 수 없는 것이다.
```

<br>

### 독립적 진화의 예시 - 웹

웹은 REST를 아주 잘 지키고 있다.

> - 웹 페이지를 변경했다고 웹 브라우저를 업데이트할 필요는 없다.
> - 웹 브라우저를 업데이트 했다고 웹 페이지를 변경할 필요는 없다.
> - HTTP 명세가 변경되어도 웹은 잘 작동한다.
> - HTML 명세가 변경되어도 웹은 잘 작동한다.

```angular2html
chrome 서버가 업데이트 되었다고 해서 우리가 브라우저를 업데이트 할 필요는 없다.
또한, 우리가 브라우저를 업데이트 했다고 웹 페이지에 접속이 되지 않는 것도 아니다.
```

<br>

## 참고

1. [Naver D2: 그런 Rest API로 괜찮은가](https://www.youtube.com/watch?v=RP_f5dMoHFc)
2. [우아한 테크: Rest API](https://www.youtube.com/watch?v=Nxi8Ur89Akw&t=628s)
3. [Rest API란?](https://hanamon.kr/rest-api/)
4. [Restful-API란?](https://velog.io/@somday/RESTful-API-%EC%9D%B4%EB%9E%80)
5. [HTTP 메소드의 멱등성](https://mangkyu.tistory.com/251)
