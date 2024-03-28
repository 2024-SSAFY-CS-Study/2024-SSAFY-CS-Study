# JVM Garbage Collector

## JVM(Java Virtual Machine)이란?
운영체제의 메모리 영역에 접근하여 메모리를 관리하는 프로그램
**메모리 관리, Garbage Collector 수행**

## Garbage Collector란?
동적으로 할당한 메모리 영역 중 사용하지 않는 영역을 탐지하여 해제하는 기능

## Stack, Heap
메모리 영역은 두가지로 나뉜다

1) **Stack**
- 정적으로 할당한 메모리 영역
- 원시 타입의 데이터가 값과 함께 할당.
- Heap 영역에 생성된 Object 타입 데이터의 참조 값 할당

2) **Heap**
- 동적으로 할당한 메모리 영역.
- 모든 Object 타입의 데이터가 할당.
- Heap 영역의 Object를 가리키는 참조 변수가 Stack에 할당

```java
public class Main {
    public static void main(String[] args) {
        int num1 = 10;
        int num2 = 5;
        int sum = num1+num2;
        String name = "던"

        System.out.println(sum);
        System.out.println(name);
    }
}

```

위와 같은 코드 실행 시 다음과 같이 각각의 영역에 할당 된다.
<img src = "/src/stack&heap.jpg">

이후에 main method가 끝나게 되면 stack 영역의 메모리는 할당이 해제되고, Heap 영역의 객체 타입 데이터만 남게 된다.
 이를 **Unreachable Object**라 불리며, Garbage Collector의 대상이 된다.
<img src = "/src/unreachableData.jpg">

## JVM의 Heap

JVM의 Heap 영역은 처음 설계될 때 2가지 전재로 설계되었다.
- 대부분의 객체는 금방 Unreachable한 상태가 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 드물다.

이에 따라 객체의 생존기간에 따라 물리적인 Heap 영역을 **Young, Old** 총 2가지 영역으로 나누어 설계했다.

<img src = "/src/heap.jpg">

- **Young Generation**
    1) 새롭게 생성된 객체가 할당되는 곳
    2) 대부분의 객체가 Unreachable 상태가 되기에, 많은 객체가 Young 영역에 생성되었다가 사라진다.
    3) 해당 영역에 대한 GC를 Minor GC or Young GC라고 부른다.

- **Old Generation**
    1) Young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
    2) Young 영역보다 크게 할당되며, 큰 만큼 GC는 적게 발생한다
    => Young 영역의 객체들은 수명이 짧기에 큰 공간 필요로 x
    3) 해당 영역에 대한 GC를 Major GC 혹은 Full GC라고 부름

<img src = "/src/card.jpg">

- **Card Table**

    - Old영역에 있는 객체가 Young영역의 객체를 참조할 때마다 그에 대한 정보가 표시
    -  Young 영역에서 가비지 컬렉션이 진행 될 때 카드 테이블만 조회하여 GC의 대상인지 식별 할 수 있도록 하는 편의성
<br>

## Garbage Collector 과정

Young, Old 영역의 공통적인 단계에는 **Stop The World**와 **Mark and Sweep**단계가 있다.

### Stop The World
- GC를 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업. 
- GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되고, GC가 끝나며 재개

### Mark & Sweep
1) Garbage Collector가 Stack의 모든 변수를 스캔하면서 각각 어떤 객체를 참조하고 있는지 마킹

2) Reachable Object가 참조하고 있는 객체도 찾아서 마킹

3) 마킹되지 않은 객체를 Heap에서 제거

<span style='background-color: #fff5b1'>**이때 1,2번 과정을 Mark, 3번 과정을 Sweep이라하여, 위 이 과정을 Mark & Sweep이라 한다.**</span>

<hr>

### Minor GC
<img src ="/src/minor.jpg">
<br>
Young 영역은 1개의 Eden과 2개의 Survivor영역 총 3구역으로 나뉜다

- **Eden**
    - 새로 생성된 객체가 할당 되는 영역
- **Survivor**
    최소 1번의 GC 이상 살아남은 객체가 존재하는 영역

#### 동작 과정

1) 새로운 객체는 Eden 영역에 할당
2) Eden 영역이 꽉차게 되고 Minor GC 실행
    1) Eden 영역에서 사용되지 않는 객체의 메모리가 해제된다.
    2) Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동
3) 1~2번 과정이 반복되다가 Surivivor 영역이 가득 차게 되면 Suvivor 영역의 살아남은 객체를 다른 Survovivor 영역으로 이동시킨다.
❗ 1개의 Survivor 영역은 반드시 빈 상태가 된다. [ 단, 객체의 크기가 Servior 영역의 크기보다 큰 경우에 바로 Old 영역으로 이동.]
4) 이러한 과정을 반복하여 계속해서 살아남은 객체는 Old 영역으로 이동된다.

이때 객체의 생존 횟수를 카운트하기 위해 Minor GC에서 객체가 살아나은 횟수를 의미하는 **age를 Object Header에 기록**
    => Minor GC때 Object Header에 기록된 age를 보고 이동여부를 결정한다.

<hr>

### Major GC
객체들이 계속 이동되어 **Old 영역의 메모리가 부족**해지면 발생

#### Minor GC와 Major GC의 영향
Young 영역은 일반적으로 Old 영역보다 크기가 작기 때문에 GC가 보통 0.5초에서 1초 사이에 끝난다. 

==> <span style='background-color: #fff5b1'>**Minor GC는 어플리케이션에 크게 영향을 주지 않는다.**</span>

Old 영역은 Young 영역보다 크며 Young 영역을 참조할 수도 있다.

==><span style='background-color: #fff5b1'>**Majer GC는 일반적으로 Minor GC보다 오래걸리며, 10배 이상의 시간을 사용한다.**</span>

<hr>

## Garbage Collection 방식

### Serial GC

#### Young 영역
- Mark & Sweep 알고리즘대로 수행

#### Old 영역
- **Mark Sweep Compact** 알고리즘 이용
- 기존 Mark & Sweep에 Compact 작업 추가
- Compact 작업은 힙의 가장 앞부분부터 채워서 객체가 존재하는 부분과 존재하지 않는 부분으로 나누는 것
<img src = "/src/msc.PNG">
❗ 서버의 CPU 코어가 1개 일 때 사용하기 위해 개발되었으며, 모든 GC 일을 처리하기 위해 1개의 쓰레드만을 이용한다.

<hr>

### Parallel GC
- Throughput GC로도 알려져 있으며, **기본적인 처리과정은 Serial GC와 동일**
- 여러 개의 쓰레드를 통해 Parallel하게 GC를 수행하기에 GC 오버헤드를 상당히 줄여줌
- 하지만 애플리케이션이 멈추는 것은 막을 수 없기에, 이를 개선하기 위해 다른 알고리즘 등장

#### Old GC
- Parallel Old GC에서는 Mark Sweep Compact가 아닌 Mark Summary Compaction 사용
- Summary 단계에서는 앞서 GC를 수행한 영역에 대해서 별도로 살아있는 객체를 식별
<img src = "/src/pogc.jpg">

<hr>

### CMS(Concurrent Mark Sweep) GC
- Parallel GC와 마찬가지로 여러 개의 쓰레드를 이용한다.
- 하지만 Mark Sweep 알고리즘을 **Concurrent(동시성, 같은 종류의 작업이 가능한 많이 동시에 일어나는것을 추구)하게 수행**
 => CMS GC가 수행될 때에는 자원이 GC를 위해서도 사용되므로 응답이 느려질 순 있지만 **응답이 멈추지는 않게 된다.**
 - 하지만, CMS GC는 다른 GC방식보다 **메모리와 CPU를 더 많이 필요**로 하며, **Compaction 단계(나누는 단계)를 수행하지 않는다**는 단점이 있다.
 => 시스템이 장기적으로 운영되다가 조각난 메모리들이 많아 **Compaction 단계가 수행되면 오히려 [Stop The World](#garbage-collector-과정) 시간이 길어지는 문제**가 발생

<hr>

## G1(Garbage First) GC
- 장기적으로 많은 문제를 일으킬 수 있는 CMS GC를 대체하기 위해 개발
-  Eden 영역에 할당하고, Survivor로 카피하는 등의 과정을 사용하지만 **물리적으로 메모리 공간을 나누지 않는다.**( Young영역과 Old영역으로 나누는 방식을 사용하지 않는다. )

### 동작 방식
- **Region(지역)** 이라는 개념을 새로 도입하여 Heap을 균등하게 여러 개의 지역으로 나누고, 각 지역을 역할과 함께 논리적으로 구분
-  Eden, Survivor, Old 역할에 더해 Humongous와 Availabe/Unused라는 2가지 역할을 추가
    - **Humongous** : Region 크기의 50%를 초과하는 객체를 저장하는 Region을 
    - **Availabe/Unused** : 사용되지 않은 Region을 의미

-<span style='background-color: #fff5b1'>**Heap을 동일한 크기의 Region으로 나누고, 가비지가 많은 Region에 대해 우선적으로 GC를 수행**</span>

<img src = "/src/g1.jpg">

#### Minor GC
한 지역에 객체를 할당하다가 해당 지역이 꽉 차면 다른 지역에 객체를 할당하고, Minor GC가 실행

1) G1 GC는 각 지역을 추적하고 있기 때문에, **가비지가 가장 많은(Garbage First)지역을 찾아서 Mark and Sweep를 수행한다**.
2) Eden 지역에서 GC가 수행되면 살아남은 객체를 식별(Mark)하고, 메모리 회수(Sweep)하고 **살아남은 객체를 다른 지역으로 이동시킨다**.
3) **복제되는 지역이 Available/Unused 지역이면 해당 지역은 이제 Survivor 영역이 되고, Eden 영역은 Available/Unused 지역이 된다**. ( 서로 상태가 바뀜)

#### Major GC
- 시스템이 계속 운영되다가 **객체가 너무 많아 빠르게 메모리를 회수 할 수 없을 때** Major GC가 실행
- 어느 영역에 가비지가 많은지를 알고 있기 때문에 **GC를 수행할 지역을 조합하여 해당 지역에 대해서만 GC를 수행**
=> 이 작업은 Concurrent하기에 **애플리케이션 지연 최소화** 가능!

❗ G1 GC는 다른 GC 방식의 처리속도를 능가하기 때문에 Java9부터 기본 가비지 컬렉터로 사용되게 되었다.


## 참고
[[10분 테코톡] 👌던의 JVM의 Garbage Collector](https://www.youtube.com/watch?v=vZRmCbl871I)

[Garbage Colletion의 개념 및 동작원리](https://bj-turtle.tistory.com/72)