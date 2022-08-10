# Part 1-3 Network

* [GET vs POST](#1-GET-vs-POST)
  * GET
  * POST

* [TCP 3-way-handshake vs TCP 4-way-handshake](#2-tcp-3-way-handshake-vs-TCP-4-way-handshake)
  * TCP 3-way Handshake
  * TCP 4-way Handshake

* [TCP vs UDP](#3-TCP-vs-UDP)
  * TCP
  * UDP

* [HTTP vs HTTPS](#4-HTTP-vs-HTTPS)
  * HTTPS

* [DNS Round Robin](#5-DNS-Round-Robin)

* [웹 통신 과정](#6-웹-통신-과정)
  * 브라우저
  * 프로트콜 스택, LAN 어댑터
  * 허브, 스위치, 라우터
  * 액세스 회선, 프로바이더
  * 방화벽, 캐시서버
  * 웹 서버

[뒤로](https://github.com/YG-creator/Interview_Question_for_Beginner)



# 1. GET vs POST



## GET

* HTTP Request Message`의 Header 부분에 url 이 담겨서 전송된다. (? 뒤에 데이터가 붙어서 전송된다.)
* 데이터 크기 제한적
* url이 노출되서 보안 약함
* 데이터를 읽어올 때 사용



## POST

* `HTTP Request Message`의 Body 부분에 데이터가 담겨서 전송된다.
* GET보다 데이터 크기가 큼
* url 노출이 안되서 GET보다 보안이 좋음
* 데이터를 변경하거나 추가할 때 사용



# 2. TCP 3-way-handshake vs TCP 4-way-handshake



## TCP 3-way Handshake

1. 사용처
   * Client와 Server 연결할 때 사용


![image-20220331190014861](C:\Users\YG\AppData\Roaming\Typora\typora-user-images\image-20220331190014861.png)

2. 연결 과정
   1) Client가 SYN M 패킷(요청) -> Server
   1) Server가 ACK M+1 패킷(응답), SYN N(요청) -> Client
   1) Client가 ACK N+1 패킷(응답) -> Server 연결 성립


3. 패킷관련 용어

   * Code Bit : 패킷 종류 명시

     * TCP Header에 위치

     * 6Bit (**Urg-Ack-Psh-Rst-Syn-Fin**)

       ex) 000010 -> SYN 패킷


   * SYN(synchronize sequence number) 
     * 요청하는 패킷
     * sequence number은 랜덤한 숫자로 해서 과거에 사용된 포트를 다시 사용할 가능성을 낮춘다.


   * ACK(acknowledgement)
     * 응답하는 패킷


4. 2-way 대신 3-way 쓰는 이유
   * 양방향(Client to Server, Server to Client)으로 존재를 알리고 패킷을 보낼 수 있는지 확인작업이 필요해서



## TCP 4-way Handshake

1. 사용처
   * Client와 Server 연결 해제할 때 사용


![image-20220331190210993](C:\Users\YG\AppData\Roaming\Typora\typora-user-images\image-20220331190210993.png)

2. 과정

   1) 클라이언트가 연결을 종료하겠다는 **FIN플래그**를 전송한다.

   2) 서버는 클라이언트의 요청**(FIN)**을 받고 알겠다는 확인 메세지로 **ACK**를 보낸다.

      2-1) 그리고나서는 데이터를 모두 보낼 때까지 잠깐 **TIME_OUT**이 된다.

   3) 데이터를 모두 보내고 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 **FIN 플래그**를 전송한다

   4) 클라이언트는 **FIN 메세지**를 확인했다는 **메세지(ACK)**를 보낸다.

   5) 클라이언트의 **ACK 메세지**를 받은 서버는 소켓 **연결을 close**한다.

   6) 클라이언트는 아직 서버로부터 받지 못한 데이터가 있을 것을 대비해 일정 시간 동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정을 거친다.**(TIME_WAIT)**




# 3. TCP vs UDP

## TCP(Transmission Control Protocol)

* **신뢰성** 과 **순차적인 전달** 을 필요로 할 때 사용
* 종단간 바이트 스트림 전송 -> 신뢰성
* 3 - way handshake로 연결 설정
* 전이중(전송이 양방향으로 동시에 일어날 수 있음)
* 점대점(2개의 종단점 있음)
* 멀티캐스팅이나 브로드캐스팅을 지원하지 않는다

## UDP(User Datagram Protocol)

* **비연결형 프로토콜** 이다.

* 포트들을 사용하여 IP 프로토콜에 인터페이스를 제공

* 서버로 짧은 요청을 보내고, 짧은 응답을 기대할 때 사용

# 4. HTTP vs HTTPS

## HTTPS

* HTTPS = HTTP(HTTP Message) + SSL(암호화)

* HTTP의 문제점 해결

  - TCP/IP 는 도청 가능한 네트워크이다. (암호화)

  - 통신 상대를 확인하지 않기 때문에 위장이 가능하다. (증명서)

  - 완전성을 증명할 수 없기 때문에 변조가 가능하다. (인증+암호화)

* 암호화에 리소스를 더 소비해서 느려지지만 요즘은 상관없어서 무조건 사용함



# 5. DNS round robin 

1. 문제점
   1. 서버의 수 만큼 공인 IP 주소가 필요함 
   2. 균등하게 분산되지 않음(캐싱 때문에 같은 서버로 접속됨) 
   3. 서버가 다운되도 확인 불가


2. 해결법

   1. Weighted round robin (WRR)

      각각의 웹 서버에 가중치를 가미해서 분산 비율을 변경한다. 물론 가중치가 큰 서버일수록 빈번하게 선택되므로 처리능력이 높은 서버는 가중치를 높게 설정하는 것이 좋다.

   2. Least connection

      접속 클라이언트 수가 가장 적은 서버를 선택한다. 로드밸런서에서 실시간으로 connection 수를 관리하거나 각 서버에서 주기적으로 알려주는 것이 필요하다.




# 6. 웹 통신 과정

## 브라우저

1. url 에 입력된 값을 브라우저 내부에서 결정된 규칙에 따라 그 의미를 조사한다.
2. 조사된 의미에 따라 HTTP Request 메시지를 만든다.
3. 만들어진 메시지를 웹 서버로 전송한다.(메시지 전송은 OS가 함)



## 프로토콜 스택, LAN 어댑터

* 프로토콜 스택은 통신 중 오류가 발생했을 때, 이 제어 정보를 사용하여 고쳐 보내거나, 각종 상황을 조절하는 등 다양한 역할을 하게 된다.

1. 프로토콜 스택(운영체제에 내장된 네트워크 제어용 소프트웨어)이 브라우저로부터 메시지를 받는다.
2. 브라우저로부터 받은 메시지를 패킷 속에 저장한다.
3. 그리고 수신처 주소 등의 제어정보를 덧붙인다.
4. 그런 다음, 패킷을 LAN 어댑터에 넘긴다.
5. LAN 어댑터는 다음 Hop의 MAC주소를 붙인 프레임을 전기신호로 변환시킨다.
6. 신호를 LAN 케이블에 송출시킨다.



## 허브, 스위치, 라우터

1. LAN 어댑터가 송신한 프레임은 스위칭 허브를 경유하여 인터넷 접속용 라우터에 도착한다.
2. 라우터는 패킷을 프로바이더(통신사)에게 전달한다.
3. 인터넷으로 들어가게 된다.



## 액세스 회선, 프로바이더

1. 패킷은 인터넷의 입구에 있는 액세스 회선(통신 회선)에 의해 POP(Point Of Presence, 통신사용 라우터)까지 운반된다.
2. POP 를 거쳐 인터넷의 핵심부로 들어가게 된다.
3. 수 많은 고속 라우터들 사이로 패킷이 목적지를 향해 흘러가게 된다.



## 방화벽, 캐시서버

1. 패킷은 인터넷 핵심부를 통과하여 웹 서버측의 LAN 에 도착한다.
2. 기다리고 있던 방화벽이 도착한 패킷을 검사한다.
3. 패킷이 웹 서버까지 가야하는지 가지 않아도 되는지를 판단하는 캐시서버가 존재한다.



## 웹 서버

1. 패킷이 물리적인 웹 서버에 도착하면 웹 서버의 프로토콜 스택은 패킷을 추출하여 메시지를 복원하고 웹 서버 애플리케이션에 넘긴다.
2. 메시지를 받은 웹 서버 애플리케이션은 요청 메시지에 따른 데이터를 응답 메시지에 넣어 클라이언트로 회송한다.
3. 왔던 방식대로 응답 메시지가 클라이언트에게 전달된다.
