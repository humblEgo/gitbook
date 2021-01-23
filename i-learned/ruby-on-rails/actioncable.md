---
description: >-
  Action Cable seamlessly integrates WebSockets with the rest of your Rails
  application. It's a full-stack offering that provides both a client-side
  JavaScript framework and a server-side Ruby framework
---

# ActionCable

Action cable\(이하 액션케이블\)은 [websocket](https://en.wikipedia.org/wiki/WebSocket)을 자연스럽게 레일즈에서 다룰 수 있게 도와줍니다.

### WebSocket이란?

#### WebSocket의 목적

WebSocket 기술의 목적을 알기 위해서는 우선 그 등장 배경을 알 필요가 있습니다.

정통적인 HTTP 프로토콜에서는 클라이언트 측에서 요청하면 서버가 수동적으로 응답하는 식으로 단방향 통신만 할 수 있었습니다. 이 경우 서버가 데이터를 업데이트하더라도 클라이언트 측에서 서버에 재요청하지 않는한 이미 제공받은 데이터를 업데이트 할 수 없었습니다. 때문에 비동기 통신이 불가능했었죠.

후에 Ajax 기술로 이런 문제를 해결하고 비동기 통신을 어느 정도 구현해냈습니다만, HTTP 자체가 가지는 한계로 인해 여전히 비효율이 존재했습니다.

가령 Ajax로 비동기 통신을 구현하기 위한 테크닉으로 폴링\(Polling\)과 롱폴링\(Long Polling\)이 있었는데 둘 모두 **모두 서버 사이드에서 업데이트가 없는데도 클라이언트가 요청을 보내야하는 비효율이 존재했습니다.** 폴링 방식은 화면을 업데이트하는 함수를 일정시간마다 동작시키는 식이고 롱 폴링 방식은 데이터 응답과 동시에 바로 새 접속을 만드는 식이기 때문입니다.

그리고 HTTP 통신에서는 요청과 응답에 메세지 헤더를 반드시 포함시켜야 했기 때문에 **겨우 1바이트의 정보를 송수신하고 싶을 때도 긴 헤더를 줄줄이 주고 받아야하는 비효율도 남아있었죠.**

그래서 이런 문제를 해결하고자 등장한 **'비동기 통신을 위한 프로토콜'이 Websocket\(이하 웹소켓\)**입니다. 웹소켓은 접속 확립에 HTTP를 사용하지만, 그 후 통신은 웹소켓 독자 프로토콜로 이루어집니다. 이 때 서버 측에서도 클라이언트 측에 통신을 먼저 보낼 수 있으며 클라이언트와 서버간의 지속적인 연결이 보장됩니다. 그래서 SNS나 구글Doc 같은 Real-time web applicatoin 구현을 위해 널리 사용되고 있죠.

웹소켓과 액션케이블은 공통적으로 쓰는 용어가 몇가지 있으니 짚고 넘어갑시다.

#### Connections

Connections\(이하 커넥션\)은 클라이언트와 서버간의 관계를 뜻합니다. 하나의 액션케이블 서버는 다수의 커넥션 인스턴스를 다룰 수 있습니다. 그리고 웹소켓 커넥션은 하나의 커넥션 인스턴스당 하나씩 할당됩니다. 참고로 물리적인 유저가 한 명이더라도 여러 개의 디바이스로 애플리케이션에 접속하면서 웹소켓을 접속한 디바이스 수만큼 열 수 있습니다. ex\) 휴대폰과 맥북으로 슬랙에 한번에 접속한 경우

#### Consumers

웹소켓 커넥션의 클라이언트를 Consumer\(이하 컨슈머\)라고 부릅니다. 액션 케이블에서는 클라이언트 사이드의 자바스크립트 프레임워크로 구현됩니다.

#### Channels

각 컨슈머는 다수의 Channel\(이하 채널\)을 구독할 수 있습니다.

#### Subscribers

컨슈머가 채널을 구독하면 그 때부터 Subscriber\(이하 구독자\)가 됩니다!

#### Pub/Sub

Pub/Sub는 [게시자 구독자 패턴](https://docs.microsoft.com/ko-kr/azure/architecture/patterns/publisher-subscriber)을 뜻합니다. 간단히 말해서 채널에 메세지를 발신하는 게시자가 있고, 이 메세지에 관심 있는 모든 채널 구독자의 출력 채널로 메세지를 복사해주는 패턴입니다.

#### Broadcastings

Broadcasting\(이하 브로드캐스팅\)은 서버가 채널 구독자들에게 메세지를 보내는 과정을 말합니다.

