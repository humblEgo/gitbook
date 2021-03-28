# Singleton pattern

객체가 너무 많아지면 컴퓨터 자원을 과도하게 사용하게 되고, 이는 프로그램 전체의 속도를 느리게 할 수 있다.

-&gt; 개발자는 객체의 최대 개수를 제한할 필요가 생긴다.

싱글턴 패턴은 **최대 N개로 객체 생성을 제한하는 패턴**이다. 이 때 중요한 것은 생성되는 객체의 최대 개수를 제한하는 데 있어서 **객체의 생성을 요청하는 쪽에서는 일일이 신경쓰지 않아도 되도록 만들어주는 것**이다.

### 싱글턴 패턴 예시

자바 프로그래밍

* 데이터베이스 커넥션 풀
* 로그 라이터

게임 프로그래밍

* 사운드 매니저
* 스코어 매니저

### 싱글턴 패턴 구현

유틸리티 메서드로 싱글턴을 구현하자. 이 때 유틸리티 메서드 외에 생성자를 직접 호출해서 객체를 생성하는 것을 막기 위해서는 생성자를 private로 설정하면 된다.

```text
   public static Database getInstance(String name) {
        if (singleton == null) {
            singleton = new Database(name);
        }
        return singleton;
    }
```

문제는 위처럼 구현할 경우, 멀티쓰레드 환경에서는 객체를 하나만 생성한다는 것을 보장할 수 없다. 쓰레드가 singleton 객체가 null인지 확인하는 과정이 거의 동시에 이뤄질 수 있기 때문이다. 이 취약점은 어떻게 해결할까?

```text
   public synchronized static Database getInstance(String name) {
        if (singleton == null) {
            singleton = new Database(name);
        }
        return singleton;
    }
```

위처럼 `synchronized`를 예약어로 넣어주면 해결된다.

하지만 이건 조금만 생각해보면 비용이 너무 비효율적인 점을 알 수 있다.

* 최초에 singleton이 null인 경우에만 다른 쓰레드와의 경합을 막으면 충분한데, `synchronized`를 예약어를 넣으면 모든 쓰레드를 기다리게하여 병목현상이 일어난다.

따라서 아래처럼 클래스 내부에서 최초에 객체 인스턴스를 만들도록 해서 개선할 수 있다.

```text
public class Database {
​
    private static Database singleton = new Database("product");
    private String name;
​
    private Database(String name) {
        try {
            Thread.sleep(100);
            this.name = name;
        } catch (Exception e) {
        }
    }
​
    public static Database getInstance(String name) {
        return singleton;
    }
```

