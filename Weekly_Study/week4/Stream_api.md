## Stream
### 정의
* 스트림(Stream)의 어휘적인 의미는 '흐름'
* 데이터의 성격을 정적/동적 두 가지 특성으로 나뉜다고 할 때...[참고](https://documentation.sas.com/doc/en/esptex/5.2/streaming-static.htm)
  * 정적인 데이터를 Static Data라 표현을 하고, 특정 시점
  * 시간에 따라 변경이 되는 데이터, 또는 일정한 흐름과 변화를 가진 데이터의 특성을 Streaming Data라고 표현을 합니다.
* Network , File 입출력과 같이 Memory Layer와 I/O 장비간에 데이터 교류를 위해 흔히 "스트림을 생성한다/닫는다" 라는 표현을 쓰기도 합니다.
* 일반적으로 우리가 논의하는 ```Stream```은 Java 8 부터 기본 스펙에 포함된 ```java.util.stream```과 이에 대한 API 명세에 한정합니다.

## Reactive Stream???
* Java9 부터 기본 스펙에 도입된 일종의 비동기(asynchronous) 스트림 처리에 대한 기술 명세 입니다.
  * 실시간성으로 흘러가는, 변화가 있는 Stream 데이터를 처리하는 프로세스를 개선하고 효율을 높이는 차원에서 도입이 되었습니다.
  * 기존에 Blocking(스레드 작업이 도중에 지연이 생기는 작업 형태)
* 핵심은 ```논 블로킹(Non-Blocking)```과 ```배압(Back Pressure)```이라고 하는 요소
* 대표적인 Java 진영의 구현체로 ```Project Reactor``` , ```RxJava```
    * reactor를 기반으로한 netty 라이브러리인 ```reactor-netty```를 서블릿 대체제로 적용된 제품이 Spring Webflux....

### Refrence
* [More Java 17 : An In-Depth Exploration of the Java Language and Its Features](https://link.springer.com/book/10.1007/978-1-4842-7135-3)
* https://www.reactive-streams.org/