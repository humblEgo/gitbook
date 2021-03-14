# DB Transaction

## 트랜잭션\(Transaction\)?

### 정의

* 트랜잭션은 **데이터베이스 상태를 변화시키기 위해서 수행하는 작업의 단위를 뜻하고, 이 작업의 완전성을 보장해주는 기능**이다.
* 데이터베이스 상태를 변화시킨다는 것은 CRUD를 위해 데이터베이스에 접근하는 것을 의미한다.
* 사용자 입장에서는 작업의 논리적인 단위로 이해할 수 있고, 시스템의 입장에서는 데이터들을 접근 또는 변경하는 프로그램의 단위가 된다.

### 특징

* 원자성\(Atomicity\)
  * 트랜잭션이 데이터베이스에 모두 반영되거나, 전혀 반영되지 않아야한다는 것
* 일관성\(Consistency\)
  * 트랜잭션의 작업 처리 결과가 항상 일관성이 있어야 한다는 것
  * 트랜잭션이 진행되는 동안에는 데이터베이스가 변경 되더라도 업데이트된 데이터베이스로 트랜잭션이 진행되는 것이 아니라, **처음에 트랜잭션을 진행하기 위해 참조한 데이터베이스로 진행된다.**
* 독립성\(Isolation\)
  * 둘 이상의 트랜잭션이 동시에 실행되고 있을 경우 **어떤 하나의 트랜잭션이라도, 다른 트랜잭션의 연산에 끼어들 수 없다는 점을 가리킨다.** 각각의 트랜잭션은 서로 간섭없이 독립적으로 수행되어야 한다.
* 지속성\(Durability\)
  * 트랜잭션이 **성공적으로 완료 되었을 경우, 영구적으로 반영되어야 한다는 점이다.**

## **트랜잭션 vs Lock**

**잠금\(Lock\)은 동시성을 제어하기 위한 기능이고, 트랜잭션은 데이터의 정합성을 보장하기 위한 기능이다.** 트랜잭션은 데이터에 대한 액세스를 예비할당\(reservation\)하는데, 이 예비할당을 잠금이라고 한다.

* 잠금은 여러 커넥션에서 동시에 동일한 자원\(record or table\)을 요청할 경우, 순서대로 한 시점에는 하나의 커넥션만 변경할 수 있게 해준다.
* 트랜잭션은 하나의 논리적인 작업 셋 자체가 완전히 적용될 수 있음을 보장한다.

실제로 내가 진행한 Rails 프로젝트에서는 트랜잭션의 특징 중 '독립성'을 잘못 이해한 나머지 record 동시접근을 막기 위해 `ActiveRecord::Base transaction` 문을 선언했었다가 시행착오를 겪고 Record 자체에 lock을 거는 형식으로 수정했다.

잠금은 아래 두 종류로 나뉜다.

* 읽기 잠금\(read lock\)과 쓰기 잠금\(write lock\)이 있다. 각각 데이터를 읽기 전에, 데이터를 쓰기 전에 잠금을 설정한다.
* **읽기 잠금은 쓰기 잠금과 충돌**을 일으키며, **쓰기 잠금은 읽기 잠금 및 쓰기 잠금과 충돌**을 일으킨다.

### **트랜잭션의 상태**

트랜잭션의 상태는 아래 5개로 나눌 수 있다.

* Active: 트랜잭션의 활동 상태. 트랜잭션이 실행 중이며 동작 중인 상태를 말한다.
* Partially Committed: 트랜잭션의 `Commit` 명령이 도착한 상태. 트랜잭션의 `commit` 이전 `sql` 문이 수행되고, `commit` 만 남은 상태를 말한다.
* Failed: 트랜잭션 실패 상태. 트랜잭션이 더 이상 정상적으로 진행할 수 없는 상태를 말한다.
* Committed: 트랜잭션 완료 상태. 트랜잭션이 정상적으로 완료된 상태를 말한다.
* Aborted: 트랜잭션이 취소된 상태. 트랜잭션이 취소되고 트랜잭션 실행 이전 데이터로 돌아간 상태를 말한다.

`Commit` 요청이 들어오면 상태는 `Partial Commited` 상태가 된다. 이후 `Commit`을 문제 없이 수행할 수 있으면 `Committed` 상태로 전이되고, 만약 오류가 발생하면 `Failed` 상태가 된다. 즉, `Partial Commited` 는 `Commit` 요청이 들어왔을 때를 말하며, `Commited` 는 `Commit`을 정상적으로 완료한 상태를 말한다.

### **트랜잭션을 사용할 때의 주의할 점**

트랜잭션은 아래 이유에서 꼭 필요한 최소의 코드에만 적용하는 것이 좋다.

* 일반적으로 데이터베이스 커넥션은 개수가 제한적이다. 그런데 각 단위의 프로그램이 커넥션을 소유하는 시간이 길어진다면 사용 가능한 여유 커넥션의 개수가 줄어들게 된다. 이는 '교착 상태'를 유발할 수 있다.

### **트랜잭션에서 교착상태\(Deadlock\)?**

뮤택스나 세마포어를 다룰 때 만났던 그 교착 상태\(Deadlock\)가 맞다. 두 개의 트랜잭션이 특정 자원\(record 또는 table\)의 lock을 획득한 채 다른 트랜잭션이 소유하고 있는 잠금을 요구하면 아무리 기다려도 상황이 바뀌지 않는 '교착 상태'에 빠질 수 있는 것이다. [예시](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database#%EA%B5%90%EC%B0%A9%EC%83%81%ED%83%9C%EC%9D%98-%EC%98%88mysql)

### **교착상태의 빈도를 낮추는 방법은?**

* 트랜잭션이 잠금을 유지하는 시간을 줄인다.
  * 트랜잭션에서 실행하는 작업의 수를 줄인다.
  * 트랜잭션을 둘 이상의 더 짧은 트랜잭션으로 분할하여 자주 커밋한다.
* 정해진 순서로 테이블에 접근한다. 가령 테이블 B-&gt;A의 순으로 접근하는 트랜잭션과 테이블 A-&gt;B로 순으로 접근하는 트랜잭션을 혼재키는게 아니라 통일하는 식이다.
* 읽기 잠금 획득\(`SELECT`\)의 사용을 피한다. 가령 MySQL에서 `SERIALIZABLE` 격리 수준에서는 모든 SELECT 쿼리에 자동적으로 `LOCK IN SHARE MODE`가 덧붙여져서 실행되는 효과가 나서 읽기 잠금을 걸고 레코드를 읽게 된다.
* 한 테이블의 복수 행을 복수의 연결에서 순서 없이 갱신하면 교착상태가 발생하기 쉽다. 이 경우에는 테이블 단위의 잠금을 획득해 갱신을 직렬화하면 동시성은 떨어지지만 교착상태를 회피할 수 있다.

### **트랜잭션 교착상태 발생시 해결방법은?**

데드락과 관련되는 트랜잭션 가운데 하나를 희생자\(victim\) 삼아 abort 하는 것이다.

## **참고**

* [https://mommoo.tistory.com/62](https://mommoo.tistory.com/62)
* [interview question for beginner](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database#%EA%B5%90%EC%B0%A9%EC%83%81%ED%83%9C%EC%9D%98-%EC%98%88mysql)
* [코딩팩토리](https://coding-factory.tistory.com/226#:~:text=1.%20%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EC%9D%80%20%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%20%EC%8B%9C%EC%8A%A4%ED%85%9C,%EA%B3%BC%EC%A0%95%EC%9D%98%20%EC%9E%91%EC%97%85%EB%8B%A8%EC%9C%84%EC%9D%B4%EB%8B%A4.)
* [잠금](https://johngrib.github.io/wiki/locking/#:~:text=%EC%9D%BD%EA%B8%B0%20%EC%9E%A0%EA%B8%88%28read%20lock%29%EA%B3%BC,%EC%9E%A0%EA%B8%88%EA%B3%BC%20%EC%B6%A9%EB%8F%8C%EC%9D%84%20%EC%9D%BC%EC%9C%BC%ED%82%A8%EB%8B%A4.)

## 
