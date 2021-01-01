# 모델 개발 - Association

#### Association으로 여러 개의 테이블 처리

Association을 사용하면 여러 테이블 사이의 관계를 모델의 관계로 조작할 수 있도록 해준다.

Association 기능을 사용하려면 반드시 다음과 같은 이름 규칙을 지켜야한다.

* 외부 키는 "&lt;참조 모델 이름&gt;\_id"의 형식
* 중간 테이블은 참조되는 테이블들을 "\_"기호로 연결, 이 때 연결 순서는 사전 순서

'중간테이블'이란 m:n 관계를 타나낼 때 서로의 Association을 관리하기 위한 테이블로, 결합 테이블이라고도 부른다.

참조 테이블의 종류

* 참조 대상 테이블이란 Associaion에서 주 키를 가지고 있는 테이블
* 참조 소스 테이블이란 외부 키를 가지고 있는 테이블

**참조 테이블의 정보 접근 - belongs\_to Association**

reviews 테이블이 book\_id를 외부 키로 하여 books 테이블을 참조하고 있도록 하려면, 아래처럼 작성해주자.

```text
class Review < ActiveRecord::Base
  belongs_to :book
end
```

**1:n 관계 - has\_many Association**

하나의 Book 객체에 대해 여러 개의 Review 객체가 존재한다는 의미를 나타내려면, 아래처럼 작성하자.

```text
class Book < ActiveRecord::Base
  has_many :reviews
end
```

이후에 view에서 `<% @book.reviews.each do |review| %>` 처럼 Association된 객체를 배열로 추출해올 수 있다.

관계형 데이터베이스에서 1:n 관계를 정확하게 정의하려면 has\_many와 belongs\_to 메서드를 모두 선언해줘야한다. 사실 참조 대상 - &gt;참조 소스의 접근만이라면 has\_many 메서드 선언만으로 충분하고 참조 소스 -&gt; 참조 대상으로의 접근만이라면 belongs\_to 메서드 선언만으로 충분하긴하다. 하지만 애플리케이션 확장성을 고려하여 두 가지를 모두 선언해주는 것을 추천!

**1:1 관계 표현 - has\_one Association**

1:1 관계는 샘플 데이터베이스에서 users 테이블과 authors 테이블과 같은 관계를 말한다.

가령 User와 Author를 연결한다면 아래처럼 작성한다.

```text
class User < ActiveRecord::Base
  has_one :author
end
```

```text
class Authro < ActiveRecord::Base
  belongs_to :user
end
```

belongs\_to\(외부 키\)는 따라간다는 의미가 있으므로, 따라가는 테이블에 적용하는 것이 자연스럽다. 이 예시에서는 본문에서 저자가 아닌 사용자는 있지만, 사용자가 아닌 저자는 없다. 따라서 사용자가 주이고 저자가 따라간다고 말할 수 있다.

**m:n 관계 - has\_and\_belongs\_to\_many Association**

관계형 데이터베이스에서는 이러한 관계를 중간 테이블을 사용해 표현하는 것이 일반적이다.

가령 Book과 Author를 연결시킨다면, 아래처럼 작성한다.

```text
class Book < ActiveRecord::Base
  has_and_belongs_to_many :authors
end
```

```text
class Author <ActiveRecord::Base
  has_and_belongs_to_many :books
end
```

단, books와 users 테이블이 reviews 테이블을 사이에 두고 m:n 관계에 있는 경우 reviews 테이블도 모델로 접근할 필요가 있다.

```text
class Book < ActiveRecord::Base
  has_many :reviews
  has_many :users, through: reviews
end
```

```text
class Review < ActiveRecord::Base
  belongs_to :book
  belongs_to :user
end
```

```text
class User < ActiveRecord::Base
  has_many :reviews
  has_many :books, through: :reviews
end
```

#### Association 으로 추가되는 메서드, 옵션, 이름 변경, counter\_cache 옵션

Association을 선언한다는 것은 모델에 대응되는 메서드를 자동으로 추가한다는 의미이기도 하다. `belongs_to`, `has_one` 로 Association을 생성할 때나 `has_mnay`, `has_and_belongs_to_many` 로 Association을 생성할 때 추가되는 메서드가 다르니 잘 체크해서 쓰자.

또한 Association을 만들 때 옵션을 지정해주면 Association 이름을 변경하거나 counter\_cache 옵션을 끄고 켜는 등의 조작이 가능하다.

