# 프로토타입

### 상속

 C\#이나 C++과 같은 클래스 기반\(class-based\)의 객체 지향 언어와는 달리 자바스크립트는 프로토타입 기반\(prototype-based\)의 객체 지향 언어이다.

따라서 상속 등의 개념이 좀 다른데, 자바스크립트에서는 현재 존재하고 있는 객체를 프로토타입으로 사용하여, 해당 객체를 복제하여 재사용하는 것을 상속이라고 한다.

### 프로토타입

자바스크립트의 모든 객체는 프로토타입\(prototype\)이라는 객체를 가지고 있다. 모든 객체는 그들의 프로토타입으로부터 프로퍼티와 메소드를 상속 받는다.

이처럼 자바스크립트의 모든 객체는 최소한 하나 이상의 다른 객체로부터 상속을 받으며, 이때 상속되는 정보를 제공하는 객체를 프로토타입\(prototype\)이라고 한다.

아 단 하나, Object.prototype 객체는 어떠한 프로토타입도 가지지 않으며, 아무런 프로퍼티도 상속받지 않는다.

### 프로토타입 체인\(prototype chain\)

자바스크립트에서는 객체 이니셜라이저를 사용해 생성된 같은 타입의 객체들은 모두 같은 프로토타입을 가진다.

또한, new 연산자를 사용해 생성한 객체는 생성자의 프로토타입을 자신의 프로토타입으로 상속받는다.

이런 프로토타입 상속의 가상 연결고리를 프로토타입 체인이라고 한다. Object.prototype 객체는 어떠한 프로토타입도 가지지 않으며, 아무런 프로퍼티도 상속받지 않는다고 했었는데, Object.prototype 객체는 이러한 프로토타입 체인에서도 가장 상위에 존재하는 프로토타입이기 때문이다.
