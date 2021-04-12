# Flyweight pattern

플라이웨이트 패턴은 비용이 큰 자원을 공통으로 사용할 수 있도록 만드는 패턴이다.

자원에 대한 비용은 크게 두 가지로 나눠볼 수 있다.

1. 중복 생성될 가능성이 높은 경우
   * 중복 생성될 가능성이 높다는 것은 동일한 자원이 자주 사용될 가능성이 매우 높다는 것을 의미한다. 이런 자원 공통 자원 형태로 관리해 주는 편이 좋다.
2. 자원 생성 비용은 큰데 사용 빈도가 낮은 경우
   * 이런 자원은 항상 미리 생성해 두는 것은 낭비이다. 따라서 요청이 있을 때에 생성해서 제공해주는 편이 좋다.

이 두 가지 목적을 위해서 플라이웨이트 패턴은 자원 생성과 제공을 책임진다.

자원의 생성을 담당하는 Factory 역할과 관리 역할을 분리하는 것이 좋을 수 있으나, 일반적으로는 두 역할의 크기가 그리 크지 않아서 하나의 클래스가 담당하도록 구현한다.

#### 장점

* 많은 객체를 만들 때 성능을 향상시킬 수 있다.
* 만흥ㄴ 객체를 만들 때 메모리를 줄일 수 있다.
* state pattern과 쉽게 결합될 수 있다.

#### 단점

* 특정 인스턴스의 공유 컴포넌트를 다르게 행동하게 하는 것이 불가능하다. \(개별 설정이 불가능하다.\)

#### 사용 예시

* 고전적인 사용 예
  * 워드 프로세서에서 문자들의 그래픽적 표현에 대한 자료 구조 \(ex 글자모양 정보를 효율적으로 다룰 수 있음\)
* JDK 예제
  * java.lang.String
  * java.lang.Integer\#valueOf\(int\) : 미리 저장된 캐시 풀에서 객체를 가지고 오고, 만약 없으면 직접 객체를 생성하는 방식으로 되어있음.
  * java.lang.Boolean\#valueOf\(boolean\)
  * java.lang.Byte\#valueOf\(byte\)
  * java.lang.Character\#valueOf\(char\)

#### 간단 구현 예시

```text
public class FlyweightFactory {
    private static Map<String, Subject> map = new HashMap<String, Subject>();
​
    public Subject getSubject(String key) {
        Subject subject = map.get(key);
​
        if (subject == null) {
            subject = new Subject(key);
            map.put(key, subject);
​
            System.out.println("새로 생성" + key);
        } else {
            System.out.println("재사용" + key);
        }
​
        return subject;
    }
}
```

