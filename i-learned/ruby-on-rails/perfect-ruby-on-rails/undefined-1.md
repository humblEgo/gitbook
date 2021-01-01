# 모델 개발

모델 클래스가 제대로 동작하는지를 간편하게 확인할 때는 액션 메서드를 일일이 만드는 대신 Rails 콘솔을 이용하면 편하다.

```text
rails console
```

`rails console --sandbox` 줄여서 `rails console -s` 로 콘솔을 실행하면 콘솔을 종료할 때 데이터베이스의 모든 변경사항을 자동으로 롤백하는 것도 가능하다.

참고로 rails runner 명령어를 사용하면 Rails 애플리케이션을 로드한 상태로 코드를 실행하는 것이 가능하다. 오래된 세션 정보를 정기적으로 삭제하는 일괄 처리 등에 사용할 수 있다. 저익적으로 자동 실행하려면 rail runner 명령어를 파일로 만들고 cron 등의 스케줄러에 등록해버리자.

```text
rails runner 'FanComment.where(deleted: true).delete_all'
```

#### where 메서드

where 메서드는 조건식에 플레이스홀더\(Placeholder\)를 사용할 수 있는 방법을 제공한다. 플레이스홀더란, 매개 변수를 두는 장소이다.

아래 예시에서 `?`부분이 플레이스홀더다. `?` 형태는 '이름 없는 매개변수'라 부른다.

```text
def ph1
  @books = Book.where('publish = ? AND price >= ?', params[:publish], params[:price])
```

**플레이스홀더를 사용하지 않고 조건식을 생성하면 SQL 인젝션 취약성의 원인이 될 수 있으니 주의하자.**

조금 코드가 길어지더라도 매개 변수의 대응 관계를 알 수 있도록 '이름 없는 매개변수' 대신 '이름 있는 매개변수'를 쓸 수도 있다.

```text
def ph1
  @books = Book.where('publish = :publish AND price >= :price', params[:publish], params[:price])
```

#### unscope 메서드

여러 개의 조건식을 추가한 경우에도 특정한 필드의 조건만 제거하는 것이 가능하다.

```text
def unscope2
  @books = Book.where(pubish: '제이펍', cd: true).order(:price).unscope(where: :cd)
  render 'books/index'
end
```

#### none 메서드

none 메서드를 쓰면 Relation\(Null Relation\) 객체를 만들어 리턴할 수 있다. 아무것도 없는 결과를 출력할 때 그냥 nil 쓰면 each 메서드 같은거 쓸 때 예외가 발생하기 쉽다.

#### pluck 메서드

여러개 필드를 배열로 추출할 수 있다!

#### 이름있는 스코프

자주 사용하는 조건을 **이름 있는 스코프\(Named Scope\)**로 지정할 수 있다.

`scope name, -> {exp}` 형식으로 아래처럼 지정하면 된다.

```text
class Book < ActiveRecord::Base
  scope :jpub, -> { where(publish: '제이펍') }
  scope :newer, -> { order(published: :desc) }
  #기존 이름 있는 스코프를 기반으로 새로 이름 있는 스코프를 생성할 수도 있다.
  scope :top10, -> { newer.limit(10) } 
end
```

쓸 때는 아래처럼!

```text
def scope
  @books - Book.jpub.top10
  render 'hello/list'
```

이름 있는 스코프에 매개 변수를 추가해서 사용할 땐 아래처럼 지정하고

```text
scope :what_new, ->(pub) {
  where(publish: pub).order(published: :desc).limit(5)
}
```

아래처럼 사용할 수 있다.

```text
@books = Book.whats_new('제이펍')
```

#### 기본 스코프 정의 - default\_scope 정의

모델 관련 메서드를 호출할 때 자동으로 조건이 적용되게 하는 기본 스코프라는 기능이 있다. `default_scope` 메서드를 이용하면 된다.

기본 스코프를 해제하고 싶은 경우에는 `unscoped` 메서드를 사용하면 된다.

#### find\_by\_sql 메서드

원칙적으로 액티브 레코드는 제공되는 쿼리 메서드를 사용해야 한다. 하지만 넘 복잡한 질의를 날려야하면 `find_by_sql`로 SQL 명령을 직접 지정한하는게 쉬울 수 있다.

SQL이 익숙하면 `find_by_sql`이 편할 수 있는데, `find_by_sql`만 사용하면 특정한 데이터베이스에 의존적이게 될 수 있으니 지양하자.

#### 트랜잭션 처리 - transaction 메서드

트랜잭션 처리는 모든 명령어의 성공 또는 실패를 한꺼번에 모아서 처리하는 것이다. 만약 일련의 명령어 중 처리 실패가 하나라도 있으면 트랜잭션에 등록되어 있던 모든 처리가 '없던 일'이 된다.

```text
def transact
  Book.transaction do
    b1 = Book.new({isbn: '123-4-1234-1234-0',
      title: 'Ruby 포켓 레퍼런스',
      price: 2000, publish: '제이펍', pulished: '2011-01-01'})
    b1.save!
  end
  render text: '트랜젝션에 성공했습니다.'
 rescue => e
  render text: e.message
end
```

트랜젝션 내부에서 레코드를 저장할 때는 `save` 메서드 보다 `save!` 메서드를 사용하는게 편하다. 전자는 성공여부를 boolean을 리턴하여 표시하지만, 후자는 예외를 발생시켜버리기 때문이다.

#### 트랜잭션 분리 레벨 지정

데이터베이스가 분리 레벨 기능을 지원하고 있다면, **트랜잭션 분리 레벨**을 지정할 수 있다. 분리 레벨은 여러 개의 트랜잭션을 동시 실행한 경우에 동작을 표시하는 것이다. 분리 레벨이 높으면 그만큼 데이터의 정합성은 높아지지만 동시 실행성은 낮아진다.

여러 개의 트랜잭션 사시에 일어나는 문제는 아래와 같다.

* 비커밋 읽기 마지막 커밋의 상태 데이터를 다른 트랜잭션이 읽어 들일 수 있음
* 비반복 읽기 여러 트랜잭션이 여러 번 동일한 데이터를 읽어 들일 때 다른 트랜잭션이 읽어들였던 값도 변화할 수 있음
* 가상 읽기 여러 번 읽어 들일 때 새로운 데이터가 발생 또는 사라질 수 있음

분리레벨 분류를 레벨이 낮은 순서로 표기하면 아래와 같다.

| 분리 레벨 | 비커밋 읽기 | 비반복 읽기 | 가상 읽기 |
| :--- | :--- | :--- | :--- |
| :read\_uncommitted | 발생 | 발생 | 발생 |
| :read\_committed | - | 발생 | 발생 |
| :repeatable\_read | - | - | 발생 |
| :serializable | - | - | - |

아래처럼 `transcation`메서드의 isolation 옵션으로 지정한다.

```text
Book.transaction(isolation: :repeatable_read) do
  @book = Book.find(1)
  @book.update(price: 30000)
end
```

