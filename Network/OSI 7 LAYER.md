# OSI 7 LAYER
- 국제 표준 기구 ISO가 발표한 네트워크 모델
- 네트워크에서 통신이 일어나는 과정을 7단계로 나눈 것

<br>
<img src = "https://camo.githubusercontent.com/ad7d385b0fdce7183bc739b1c0dd2fc28a72e5207159001cb9e6785610d5131d/68747470733a2f2f73373238302e7063646e2e636f2f77702d636f6e74656e742f75706c6f6164732f323031382f30362f6f73692d6d6f64656c2d372d6c61796572732d312e706e67">

## OSI 7 계층을 나눈 이유
- 네트워크 상에서 여러 대의 컴퓨터가 데이터를 주고받으려면 이들을 연동할 수 있도록 **표준화 된 인터페이스**가 필요!
- 통신이 일어나는 과정을 단계별로 알 수 있고, 특정한 곳에 이상이 생기면 그 단계만 수정할 수 있기 때문(**분할 정복**)
=> 계층 구조를 가지기에 다음 단계로 넘어가기 위해서는 이전 계층에서 문제가 없어야 함

### PDU?
- PDU(Process Data Unit) 란 **각 계층에서 전송되는 단위**를 말한다.
- 1 계층에서 PDU가 비트(Bit)라고 생각하기 쉽지만 PDU라고 하지 않고 여기서 비트는 단위라기보다는, 단지 전기 신호의 흐름일 뿐이다.
- 네트워크 통신 과정을 깊게 이해하기 위해서는 왜 각각의 계층의 PDU가 다른지 알아야 하고,역할에 대해 알고 있어야 한다.


## 1. 물리(Physical) 계층
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqeXJk%2FbtrD19TmojQ%2FFAK8VVKwhgBSlauKkYofk1%2Fimg.png">
<br>

- <span style='background-color: #fff5b1'>**데이터를 전기적인 신호로 변환해서 주고받는 기능을 진행하는 공간**</span>
=> 기계어를 전기적 신호로 바꿔 와이어에 실어줌
=> 단지 데이터만 전달하기에, 어떤 데이터인지, 어떤 오류가 있는지는 신경 X
- **PDU : 비트**
- 장비 : 통신 케이블, 허브 등
- 프로토콜 : Modem, Cable, Fiber 등


## 2. 데이터 링크(Data Link) 계층
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcedrlS%2FbtrD1tXvqHh%2FhOkxUgU1Qhr0RDNC6VlVx1%2Fimg.png">

- <span style='background-color: #fff5b1'>**네트워크 기기들 사이의 데이터 전송을 하는 역할**</span>
- 시스템 간의 오류 없는 데이터 전송을 위해 패킷(Packet)을 프레임(Frame)으로 구성하여 물리 계층(1 계층)으로 전송한다.
- 네트워크 계층(3 계층)에서 정보를 받아 주소와 제어 정보를 헤더와 테일에 추가한다.
- <span style='background-color: #fff5b1'> Point to Point 간 **신뢰성 있는(안전한) 전송을 보장**하기 위한 계층</span>
- 물리 계층을 통해 송수신되는 데이터의 전송 오류를 감지하는 기능을 제공, 오류 감지시 재전송
- MAC 주소를 통해 통신
- **PDU : 프레임(Frame)**
- 장비 : 브리지, 스위치 등
- 프로토콜 : 이너넷, MAC, PPP, ATM 등

## 3. 네트워크(Network) 계층
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNR9Ge%2FbtrD8MPHiQ0%2FkqBNBNRmKWf6Kwdil4mi71%2Fimg.png">

- <span style='background-color: #fff5b1'>**기기에서 데이터그램(Datagram)이 가는 경로를 설정**</span>
- 라우팅 알고리즘을 통해 최적의 경로를 선택하고 송신 측으로부터 수신 측으로 전송
- 이때 <span style='background-color: #fff5b1'>전송되는 데이터는 **패킷**단위로 분할하여 전송한 후 재조합</span>
=> 4계층에서 요구하는 서비스 품질(QoS)을 제공하기 위한 기능적, 절차적 수단 제공
- 2계층과 같이 노드 대 노드 전달이 아니라, 목적지까지 성공적이고 효과적으로 전달될 수 있도록 함.
- **PDU : 패킷(Packet)**
- 장비 : 라우터, L3 스위치
- 프로토콜 : IP,ICMP 등

## 4. 전송(Transport) 계층

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FoQpWd%2FbtrD5R5gZOF%2FUnuAxRirD1tiZcqEKuSUj1%2Fimg.png">

- <span style='background-color: #fff5b1'>**발신지에서 목적지(End to End)간 제어와 에러를 관리**</span>
- 패킷의 전송이 유효한지 확인하고 전송에 실패된 패킷을  다시 보내는 것과 같은 신뢰성 있는 통신을 보장
- 헤드에는 **세그먼트(Segment), 포트 번호** 가 포함된다. 
=> 포트 번호 : 디바이스에 있는 여러 프로세스 중 자기가 가야 할 프로세스를 구분
- 주소 설정, 오류 및 흐름 제어, 다중화를 수행한다.
- **PDU : 세그먼트(Segment)**
- 장비 : 게이트웨이, L4 스위치
- 프로토콜 : TCP, UDP, ARP, RTP

#### TCP
- 신뢰성 있는 전송 보장(패킷 손실, 중복, 순서 바뀜 등 없도록)
- IP가 처리할 수 있도록 **데이터를 여러 개의 패킷으로 나누고**, 도착지에서 완전한 데이터로 **패킷을 재조립**
- PDU : 세그먼트

#### UDP
- 비연결성, 비신뢰성 서비스
- TCP와 다르게 **패킷을 나누고 재조립하는 과정 없음**
 => 에러와 그에 따른 재전송, 대체는 애플리케이션에서 처리해야 한다.
- 하지만 속도 빠름
=> 스트리밍 사이트와 같은 RealTime서비스에 사용
- PDU : 블록형태의 다이어그램

## 5. 세션(Session) 계층
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fdz1Eix%2FbtrD9F3TOJl%2FSVMQIgJmLDK3J7yUpeSiFK%2Fimg.png">

- <span style='background-color: #fff5b1'>**통신 세션을 구성하는 계층으로, 포트 번호를 기반으로 연결**</span>
- 네트워크 상 양쪽 연결을 관리하고 연결을 지속 시켜줌
-  TCP/IP 세션을 만들고 없애는 역할
- 프로토콜: NetBIOS, SSH, TLS

## 6. 표현(Presentation) 계층

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fn2Uho%2FbtrD2IH47et%2FQpbPHsQRxlhXdQTkKNdDRK%2Fimg.png">

- <span style='background-color: #fff5b1'>**응용 계층에서 전달받거나 전송하는 데이터의 인코딩 - 디코딩 및 암호화 등이 이루어지는 계층**</span>
- 데이터의 형식상 차이를 다루는 부담을 응용 계층으로부터 덜어줌
- 프로토콜 : JPG,MPEG,SMB,AFP

## 7. 응용(Application) 계층

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeTEj5v%2FbtrD6xFx2fb%2FgJtnwbky9prdKQG2ZHOJDK%2Fimg.png">

- <span style='background-color: #fff5b1'>**응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 네트워크에 연결 및 수행하는 역할**</span>
- 사용자와 직접 접하는 유일한 계층
    - 사용자로부터 정보를 입력받아 하위 계층으로 전달하고, 하위 계층에서 전송하는 데이터를 사용자에게 전달
    - UI, I/O 부분
- 프로토콜 : HTTP, DNS, Telnet, FTP


## 전체적인 통신 플로우
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcrYJdq%2FbtrD6yRXRGA%2F5I06bKoUqZeJXqABcwIHkk%2Fimg.jpg">

1. 발신 측에서 응용 계층(7 Layer)부터 시작해 각 계층마다 헤더를 붙여 캡슐화를 진행
2. 수신 측에서는 물리 계층(1 Layer)부터 차례로 올라가면서 헤더를 떼 내는 디캡슐레이션을 진행하여 데이터 식별

    - 데이터가 목적지로 이동할 때, 네트워크 계층(3 Layer)에서 IP 헤더에 있는 프로토콜 정보를 이용해 데이터가 TCP인지 UDP인지 식별
    - 그에 따른 처리를 전송 계층(4 Layer)에서 수행한다.


3. 목적지에 원하는 데이터가 전송된다.

## TCP/IP 4계층
<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F213F623C566BAE253B">

- 실제로는 TCP/IP 4계층을 더 많이 따름
- 네트워크 엑세스 : 물리 + 데이터 링크
- 인터넷 : 네트워크
- 전송 : 전송
- 응용 : 세션 + 표현 + 응용

## +) 암기 팁
<img src = "https://github.com/ssafy-tech-concert/ssafy-tech-concert/raw/master/images/tip.jpg">






# 참고
[OSI 7 layer](https://github.com/ssafy-tech-concert/ssafy-tech-concert/blob/master/Computer-Science/OSI%207%20layer.md)
<br>
[OSI 7 계층 (OSI 7 Layer)](https://backendcode.tistory.com/167)
<br>
[TCP/IP 4계층(TCP/IP 4 Layer)](https://hahahoho5915.tistory.com/15)
<br>
[[10분 테코톡] 👍 파즈의 OSI 7 Layer](https://www.youtube.com/watch?v=Fl_PSiIwtEo)
