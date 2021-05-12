# DTO vs VO

## 차이

DTO

* 목적: 레이어 간 데이터 전달
* 동등결정: 속성값이 모두 같아도 같은 객체가 아니다.
* 가변/불변: setter 존재시 가변, 비존재 시 불변
* 로직: getter/setter 외의 로직을 갖지 않는다.

VO

* 용도: 값 표현
* 동등결정: 속성값이 모두 같으면 같은 객체
* 가변/불변: 불변
* getter/setter외의 로직을 가질 수 있다.

## DTO\(Data Transfer Object\)

계층간 데이터를 주고 받기 위한 객체

예시\) Web Layer 와 Service Layer 간에 데이터를 주고 받을 때 DTO를 쓴다.

### **특징**

* 오직 getter/setter 메서드만을 갖는다. 그 외 다른 로직을 갖지 않는다.

### DTO는 불변객체로 쓰자!

아래처럼 getter만 두고 setter는 없애며 생성자에서 셋팅하도록 하면, 데이터의 변조가 없음을 보장할 수 있다.

```text
public class CrewDto {
	private final String name;
	private final String nickname;

	public CrewDto(String name, String nickname) {
		this.name = name;
		this.nickname = nickname;
	}

	public String getName() {
		return name;
	}

	public String getNickname() {
		return nickname;
	}
}
```

### DTO Class와 Entity Class를 분리하자.

요청이나 응답 값을 전달하는 클래스로 절대로 Entity Class를 쓰면 안 된다. 데이터베이스가 맵핑되어있는 핵심클래스이기 때문이다. Entity 클래스를 기준으로 테이블이 생성되고 스키마가 변경된다.

뷰의 변경에 따라 다른 클래스에 영향을 주지 않는 DTO 클래스를 활용하는게 좋다.

## VO\(Value Object\)

값 그 자체를 표현하는 객체

실생활의 VO의 예시는 돈이다. 서로 다른 만원의 일련번호가 다르더라도 우리는 모두 만원으로 취급한다.

일련번호가 객체주소, 인스턴스가 만원이라고 생각해보자.

### 특징

* VO도 불변객체여야 한다. 별도 setter 성격의 메서드는 포함하면 안 되고, 생성자에서 모든 값을 셋팅해버린다.
* 그래도 getter/setter 외의 다른 로직을 가질 수 있다.

