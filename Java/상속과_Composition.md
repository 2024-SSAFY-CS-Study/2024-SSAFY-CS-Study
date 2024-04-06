# 상속과 Composition



객체지향 프로그래밍에서 상속이나 Composition을 통해 확장을 용이하게 하고, 코드의 재사용성을 높일 수 있다. 

<br/><br/>

## 객체지향 프로그래밍에서 확장하는 방법

1. 상속
2. 조합

<br/>

## 상속


### 장점

- 코드의 재사용성이 높아진다
- 다형성을 지킬 수 있다

<br/>

### 단점

- 유연성이 떨어진다
- 오버라이딩 시 문제여지가 있다
  - 캡슐화 문제
  - LSP(리스코프 치환원칙) 위배 가능성

<br/><br/>

## Composition(조합)


### 장점

- 유연성을 제공할 수 있다
- 다중상속을 해소할 수 있다
- 불필요한 메서드 노출이 없어진다

<br/>

## 상속과 조합의 예시

```java
class Bird {
    울다() {
        
    }
    
    날다() {
        
    }
}

class 참새 extends Bird {
}
```

<br/>

```java
class WinningLotto {
    private final Lotto lotto;
    private final LottoNumber bonusNumber;
    
    public WinningLotto(Lotto lotto, LottoNumber bonusNumber) {
        this.lotto = lotto;
        this.bonusNumber = bonusNumber;
    }
}
```

<br/><br/>



## 제대로 상속하기

### 상속의 단점

```java
class Bird {
    울다() {
        
    }
    
    날다() {
        
    }
}

class 러버덕 extends Bird {
}
```

- 러버덕은 날 수 없지만 Bird를 상속받아 메서드를 가지고 있음
  - LSP위반, 부모의 역할을 제대로 수행하지않음
  - 유연성 부족

<br/>

- 부모메서드를 상속받아 사용하거나, 오버라이딩 했을 때 원하는 결과가 나오지 않을 시 내부를 뒤져봐야함
  - 캡슐화 위반

<br/>

```java
class 소나타 extends 기름엔진 {
    @Override
    public void 시동() {
        
    }
    
    @Override
    public void 드라이브() {
        
    }
}

class 소나타 extends 전기엔진 {
    @Override
    public void 시동() {
        
    }
    
    @Override
    public void 드라이브() {
        
    }
}
```

- 이름이 어중간함
- 이 경우에는 엔진을 멤버변수로 두고, 생성자에서 어떤 걸 받을지 정해주면 된다
- Composition이 유연성이 더 좋다!

<br/><br/>

## 상속은 언제 써야할까?

1. <u>**Is-a 관계가 명확할 때**</u>
   - 상속은 파급효과가 강력하기 때문에 
2. 본인의 코드일 경우
3. 상속을 위해 설계된 경우
   -  나만 사용 x 동료도 사용

<br/><br/>

## 상속을 쓰면 안되는 경우

1. 다형성을 위한 상속
   - 억지로 만들 필요 x
   - 다형성만을 너무 추구하면 안됨
2. 코드 재사용을 위한 상속

<br/><br/>

## 갑작스러운 상속을 대비하는 법

1. 클래스의 final 붙이기
   - 내부는 알 필요가 없어지고
   - 사용자들은 조합으로 클래스를 확장할 수 있음
2. 중요한 로직은 abstract로
   - 클라이언트에게 맡긴다
3. 신중하게 상속을 도입
   - **<u>클래스의 결함은 자식에게도 넘어간다</u>**
   - 조슈아 블로크의 마지막 충고

<br/>

+) 일급 컬렉션도 조합이다

<br/><br/>


## 참고자료

https://www.youtube.com/watch?v=YJ4JJsGy8rY