# Observer pattern

### 개념

한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식으로 일대다\(one-to-many\) 의존성을 정의한다.

### 자바 내장 옵저버 패턴 사용

java.util.Observer 인터페이스와 java.util.Observable 클래스를 사용할 수 있음.

* 자바 내장 옵저버 패턴은 푸시 방식, 풀 방식 모두 사용가능.

java.util.Observer 인터페이스를 구현하고 java.util.Observable 객체의 addObserver\(\) 메소드를 호출하면 옵저버 목록에 추가가 되고 deleteObserver\(\)를 호출하면 옵저버 목록에서 제거가 된다.

연락을 돌리는 방법은 java.util.Observable를 상속받는 주제 클래스에서 `setChanged()` 메소드를 호출해서 객체의 상태가 바뀌었다는 것을 알린 후 `notifyObservers()` 또는 `notifyObserver(Object arg)` 메소드를 호출하면 된다. \(인자값을 넣어주는 메소드는 푸시방식으로 쓰임.\)

옵저버 객체가 연락을 받는 방법은 `update(Observable o, Object arg)` 메소드를 구현하기만 하면 된다. `Observable o` 에는 연락을 보내는 주제 객체가 인자로 전달이 되고 `Object arg` 에는 `notifyObservers(Object arg)` 메소드에서 인자로 전달된 데이터 객체가 넘어온다.

### 자바 내장 옵저버 패턴의 단점과 한계

1. Observable은 클래스다. 따라서 재사용성에 제약이 생긴다.
   * 따라서 서브 클래스를 만들어야 한다는 점이 문제다. 이미 다른 슈퍼 클래스를 확장하고 있는 클래스에 Observable의 기능을 추가할 수 없기 때문이다. 그래서 재사용성에 제약이 생긴다.
2. Observable 클래스의 핵심 메소드를 외부에서 호출할 수 없다.
   * Observervable API를 살펴보면, `setChanged()` 메소드가 protected로 선언되어 있따.
   * Observable의 서버클래스에서만 `setChanged()`를 호출할 수 있다. 결국 직접 어떤 클래스를 만드록, Observable의 서브클래스를 인스턴스 변수로 사용하는 방법도 쓸 수 없다. 이런 디자인은 상속보다는 구성을 사용한다는 디자인 원칙에도 위배된다.

