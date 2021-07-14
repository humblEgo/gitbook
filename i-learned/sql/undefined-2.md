---
description: 동시성 제어
---

# 동시성 제어에 관하여

## 트랜잭션이 동시 접근시 발생되는 현상

### Dirty Write \(갱신내용 손실, Lost update\)

* 같은 데이터에 대해 동시에 두 개 이상의 트랜잭션이 값을 바꾸고자 할 때 발생되는 현상. 늦게 시도된 트랜잭션이 무효화된다.

### Dirty Read

* 아직 종료\(commit\)되지 않은 트랜잭션의 쓰기 내용을 읽는 것으로 비정상적 상태의 데이터를 읽게 되는 현상

### Non-repeatable Read

* 어떤 트랜잭션에서 동일한 데이터의 값을 매번 읽을 때마다 틀려지는 현상

### Phantom Read

* 기존 데이터는 동일한데 새로 추가된 값에 의해 데이터 값이 변경되는 현상.
  * 이미 존재하는 데이터의 변경, 삭제는 불가하지만 데이터 추가는 가능할 때 생기는 현상이다.

### Cascading Rollback\(연쇄 복귀\) or 회복 불능\(Unrecoverability\)

* 복수의 트랜잭션이 데이터 공유시 특정 트랜잭션이 처리를 취소할 경우 다른 트랜잭션이 처리한 부분에 대해 취소 불가능한 현상

## 동시성 제어 기법

### Locking

* 트랜잭션이 사용하는 자원\(데이터 항목\)에 대하여 상호 배제\(Mutual Exclusive\) 기능을 제공
* [자세한건 따로 정리](https://app.gitbook.com/@injun-woo30000/s/growth-log/~/drafts/-Mb0RZPo9HNAIsUTNGzw/i-learned/sql/lock)해뒀다.

### Timestamp Ordering

* DBMS가 부여하는 유일한 식별자인 타임 스탬프를 지정하여 트랜잭션 간의 순서를 미리 선택
* 트랜잭션 대기 시간이 없어서 교착상태\(Deadlock\)를 방지할 수 있다.
  * 그래서 처리 속도는 빠르지만, 다른 트랜잭션에 비해서 롤백될 확률이 높다.
* 예외가 던져지면 연쇄 복귀\(Cascading Rollback\)를 초래
* 시스템 시계 및 논리적 계수기 이용

### 낙관적 검증

* 트랜잭션 수행 동안은 어떠한 검사도 하지 않고, 트랜잭션 종료 시에 일괄적으로 검사
* 트랜잭션 수행 동안 그 트랜잭션을 위해 유지되는 데이터 항목의 지역 **사본에 대해서만 갱신**
* 트랜잭션 종료 시에 동시성을 위한 트랜잭션 **직렬화가 검증되면** 일시에 DB로 반영함.
  * 그래서 처리 속도는 빠르지만, 다른 트랜잭션에 비해서 롤백될 확률이 높다. 이게 자원낭비 가능성으로 이어지므로 동시 사용 빈도가 낮은 시스템에서 사용된다.
* **Read Phase, Validation Phase, Execution Phase로 구분**

### 다중 버전 제어기법

* 하나의 데이터 아이템에 대해 여러 버전의 값 유지
* **조회 성능을 최대한 유지하기 위한 기법 \(버전을 관리해서, 조회와 업데이트를 분리하기 때문\)**
* 트랜잭션간의 충돌 문제는 대기가 아니라 복귀처리 함으로 연쇄 복귀 초래 발생 가능성

## 참고

* [https://reiphiel.tistory.com/entry/understanding-jpa-lock](https://reiphiel.tistory.com/entry/understanding-jpa-lock)
* [http://www.jidum.com/jidums/view.do?jidumId=282](http://www.jidum.com/jidums/view.do?jidumId=282)
* [https://centbin-dev.tistory.com/40](https://centbin-dev.tistory.com/40)
* [https://sabarada.tistory.com/122?category=822063](https://sabarada.tistory.com/122?category=822063)
* [https://www.youtube.com/watch?v=w6sFR3ZM64c](https://www.youtube.com/watch?v=w6sFR3ZM64c)



