## RabbitMQ

AMQP(Advanced Message Queuing Protocol, 메시지 지향 미들웨어를 위한 개방형 표준 응용 계층 프로토콜)로 구현된 Erlang과 Java로 짜여진 메시지 브로커이다. 

<br>

<p style="text-align:center;"><img src="resources/1.png" width=550></p>

<figure><p style="text-align:center;"><img src="resources/2.png"><figcaption>"P" is our producer and "C" is our consumer.</figcaption></p></figure>

가운데 빨간색 박스가 queue인데 Consumer 대신에 RabbitMQ와 같은 broker(브로커)가 킵하고 있는 메시지 버퍼의 이름(여기선, “hello”)이다. 

<aside>
💡 간단하게,<br>
1. Producer가 “hello” 라는 queue에 메시지를 보낸다(발행한다).<br>
2. Consumer는 해당 queue에 있는 메시지를 받는다.
</aside>

<br>

### **Work(Task) Queues**

<p style="text-align:center;"><img src="resources/3.png"></p>

Work(Task) Queues는 여러 workers를 둠으로써 시간이 걸리는 작업들을 분산시킬 수 있다. Task를 스케줄하여 나중에 완료시킬 수 있다. Task를 메시지로 캡슐화하여 queue로 보낸다. 

<aside>
💡 이 개념은 특히, 짧은 HTTP 요청 윈도우동안 복잡한 task를 처리할 수 없는 웹 애플리케이션에서 유용하다.
</aside>

<br>

> **라운드-로빈 배정(Round-robin dispatching)**
> 

Work(Task) Queue를 이용하는 장점들 중 하나는 일을 쉽게 병렬 처리할 수 있다는 점이다. RabbitMQ는 기본적으로(By default) 각 메시지를 다음 consumer에게 순서대로 보낸다. 평균적으로 각 consumer는 동일한 갯수의 메시지를 받는다. 이렇게 메시지들을 분산시키는 과정을 라운드-로빈(round-robin)이라 한다.

<br>

> **Message acknowledgment**
> 

Consumer가 메시지를 가져가는 처리 작업 도중에 죽거나 할 때, 메시지를 잃지 않기 위해 RabbitMQ는 message acknowledgments라는 것을 제공한다. Consumer는처리 완료된 특정 메시지를 지워도 된다는 ack를 RabbitMQ에게 다시 보내어 메시지를 잃는 상황을 방지한다. 

만약 consumer가 ack를 보내기 전에 처리 도중 연결이 끊긴다면, RabbitMQ는 메시지가 제대로 처리되지 않음을 인지하고 다시 queue에 쌓아 consumer에게 전송될 수 있도록 한다.

<br>

### **Publish/Subscribe**

수많은 consumer들에게 한 번에 메시지를 전송(브로드캐스트)하는 패턴이다.

> **Exchanges**
> 

<figure><p style="text-align:center;"><img src="resources/4.png"><figcaption>“X” is an exchange.</figcaption></p></figure>

RabbitMQ의 메시징 모델에서는 producer가 절대로 어떠한 메시지든 queue에 직접적으로 보내지 않는다. 심지어 producer는 보낸 메시지가 어느 queue에 쌓이는 지도 모른다.

Producer는 대신 exchange에게만 메시지를 전송한다. Exchange의 한 쪽에서는 producer로부터 메시지를 받고 다른 한 쪽에서는 그 메세지를 queue에게 보낸다. 

- **Direct Exchange**

<p style="text-align:center;"><img src="resources/5.png"></p>

Message의 Routing Key와 정확히 일치하는 Binding된 Queue로 전달(Routing)한다. (1:1)

- **Fanout Exchange**
- **Topic Exchange**
- **Headers Exchnage**

<!-- 
```
1) Direct exchange - (Empty string) and amq.direct
 참고) AMQP 정의 : 바인딩 된 Queue 중에서 메시지의 라우팅 키와 매핑되어 있는 Queue로 메시지를 전달(1:1)
2) Fanout exchange - amq.fanout
 참고) AMQP 정의 : 메시지의 라우팅 키를 무시하고 Exchange에 바인딩 된 모든 Queue에 메시지를 전달(1:N)

3) Topic exchange -amq.topic
 참고) AMQP 정의 : Exchange에 바인딩 된 Queue 중에서 메시지의 라우팅 키가 패턴에 맞는 Queue에게 모두 메시지를 전달(Multicast)

4) Headers exchange - amq.match (and amq.headers in RabbitMQ)
 참고) AMQP 정의 : 라우팅 키 대신 메시지 헤더에 여러 속성들을 더해 속성들이 매칭되는 큐에 메시지를 전달
```
-->


### **Routing**

Receiving messages selectively

### **Topics**

Receiving messages based on a pattern

### **Remote procedure call (RPC)**

<br>

---

### **References**

*Rabbitmq tutorials*. RabbitMQ. (n.d.). Retrieved July 4, 2022, from [https://www.rabbitmq.com/getstarted.html](https://www.rabbitmq.com/getstarted.html) 

*Wikimedia Foundation.* (2022, February 14). *AMQP*. Wikipedia. Retrieved July 4, 2022, from [https://ko.wikipedia.org/wiki/AMQP](https://ko.wikipedia.org/wiki/AMQP)