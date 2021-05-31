# Redis 개요

Redis는 Remote dictionary server의 약자이다.

즉,외부에 있는 dictionary 자료구조를 사용하는 서버를 사용하는 DB라는 것이다.아래 특징이 있다.

* 메모리 상에 데이터를 저장한다.
* 다양한 자료구조를 지원한다.

## 메모리 계층 구조

위

* CPU register
* CPU Cache
  * 매우 빠르고, 매우 비싸다. -&gt; 저장공간으로 삼기엔 용량이 넘나 부족하다.
* Main Memory \(DRAM\)
  * 적당히 빠르고, 적당히 비싸다.
  * 휘발성
* Storage \(SSD, HDD\)
  * 느리고, 싸다.
  * 비휘발성

아래

위로 갈수록 빠르고 비싸고, 아래로 갈수록 느리고 싸다.

Redis는 Storage 대신 Main Memory에 데이터를 저장하면 어떨까? 하는 배경에서 나왔다.

Database보다 더 빠른 Memory에 더 자주 접근하고 덜 자주 바뀌는 데이터를 저장하자.

## Data structure

redis는 memcached와 달리 다양한 collection 자료구조를 제공한다.

* String
  * Key-Value 쌍
  * Java의 Map Entry
  * Set &lt;Key&gt; &lt;Value&gt;
  * Get &lt;Key&gt;
  * Del &lt;Key&gt;
* List
  * Key-List 쌍
* Set
* Hash-Set
* Hash

## Java의 hashMap도 메모리에 저장되는데, Redis와 다른 점이 뭐야?

Java의 hashMap에 데이터를 저장하면 아래 문제들이 예상된다.

* 서버가 여러대인 경우 Consistency의 문제가 발생할 수 있다.
* Multi-Threaded 환경에서 Race Condition 문제가 발생할 수 있다.
  * Race condition은 여러 개의 쓰레드가 경합하는 것을 말한다. 이 경우 Context switching에 따라 원하지 않는 결과가 발생할 수 있다.

Redis는 기본적으로 Single Threaded 이므로 Race condition 문제를 예방한다.

Redis 자료구조는 Atomic Critical Section에 대한 동기화를 제공한다. Critical Section은 동시에 여러 쓰레드가 접근하면 안되는 영역을 말한다.

서로 다른 Transaction Read/Write를 동기화한다.

## 어디서 쓰나요?

여러 서버에서 같은 데이터를 공유할 때

Single Server라면? Atomic 자료구조 & Cache 기능을 사용하기 위해 쓰인다.

## 주의해야할 점

Single Thread 서버 이므로 시간 복잡도를 고려해야 한다.

* 안 그러면 비싼 명령에 병목이 발생할 수 있다.

In-memory에서 동작하는 Db이므로 메모리 파편화, 가상 메모리 등의 이해가 필요하다.

* 메모리 파편화
  * 메모리를 할당받고 해지하는 과정에서 연속된 메모리 사이에 빈 공간이 생길 수 있다. 이러다보면 실제 사용하는 메모리보다 더 많은 메모리를 사용 중이라고 컴퓨터가 오해해서 전체 프로그램이 죽을 수 있다.
* 가상 메모리-스왑
  * 보통 전체 프로세스를 메모리에 올려서 사용하지 않고, 필요한 부분만 올리고 나머지는 가상 메모리에 올려놨다가 사용한다. 이 과정에서 레이턴시가 발생할 수 있고, 이건 또 싱글쓰레드라는 Redis 특성상 병목으로 작용할 수 있다.

## Replication - Fork

인메모리 DB이므로 기본적으로 메모리가 유실될 위험을 가지고 있다. 그래서 데이터를 복제 기능을 제공한다. 이 때 fork를 이용해서 복사하는데, 만약 메모리가 부족하다면 Redis가 죽어버릴 수 있다.

## 더 공부해볼 부분

Redis Persistent, RDB, AOF

Constant Hashing

Data grid  


## 참고

[https://www.youtube.com/watch?v=Gimv7hroM8A](https://www.youtube.com/watch?v=Gimv7hroM8A)

