# 모델 개발 - 마이그레이션

이그레이션은 Rails가 제공하는 테이블 레이아웃을 생성 또는 변경하기 위한 기능이다.

데이터베이스 스키마 변경을 하는 역할을 하는 것은 '마이그레이션 파일'이다. 마이그레이션 파일은 `rails generate` 명령으로 생성할 수 있다. 이 때 파일 이름에 포함되는 타임 스탬프 값이 포함되는데, Rails는 이 값을 `schema_migrations` 테이블에 저장해서 버전 관리한다.

정확히 말해서 마이그레이션 파일을 생성하는 방법은 다음과 같이 두 가지로 구분할 수 있다.

1. `rails generate model` 명령어로 모델과 함께 생성
2. `rails generate migration` 명령어를 사용해 마이그레이션 파일만 생성

2번을 자주 쓰게되는데 이건 좀 정리해두자.

```text
rails generate migration name [field:type ...] [option]
​
#name: 이름
#field: 필드 이름
#type: 자료형
#options: 동작 옵션
```

이름은 마이그레이션 파일\(ActiveRecord::Migration 파생 클래스\)의 클래스 이름이다.

각각의 필드를 추가하거나 제거하는 경우에는 다음과 같은 형식을 지정한다.

* `AddXxxxxTo<테이블 이름>` : 요기서 Xxxxx 부분은 어디까지나 이름을 읽기 위한 것이므로 추가하거나 제거하려는 필드는 객체\(&lt;필드이름&gt;:&lt;자료형&gt;\)로 별도 지정해줘야 한다.
* `RemoveXxxxxFrom<테이블 이름>`

  가령 authros 테이블에 date 자료형으로 birth 필드를 추가한다면 아래처럼 명령어를 입력하자.

  ```text
  rails generate migration AddBirthToAuthors birth:date
  ```

  위 명령어로 자동 생성되는 코드는 어디까지나 그냥 기본적인 구조를 잡아주는 것뿐이므로 추가 처리를 하고 싶다면 코드를 직접 추가하자.

  마이그레이션 파일을 명령어를 쓰지 않고 직접 생성하는 것도 가능하다. 하지만 **쓸데없는 충돌이 일어날 수 있으므로 명령어를 사용할 것을 추천한다.**

  직접 생성하거들랑 아래 사항은 꼭 주의하자.

  * 파일의 이름과 클래스 이름을 대응시킨다. &lt;-- 요거.. 중요하다! 타임스태프도 꼭 붙여줘야한다!
  * ActiveRecord::Migration 클래스를 상속한다.

  마이그레이션 파일에서 다룰 수 있는 메서드들이 따로 있으니 이를 [확인](https://api.rubyonrails.org/v6.1.0/classes/ActiveRecord/ConnectionAdapters/SchemaStatements.html#method-i-add_column)해서 잘 써먹자.

  주요 메서드

  * add\_column
  * add\_index
  * add\_timestamps
  * change\_column
  * change\_column\_default
  * change\_table
  * column\_exists?
  * create\_table
  * create\_join\_table
  * drop\_table
  * index\_exists?
  * remove\_column
  * remove\_index
  * remove\_timestamps
  * rename\_column
  * rename\_index
  * rename\_table
  * execute

  **마이그레이션 파일 실행**

  마이그레이션 파일을 실행할 때는 rake 명령어를 사용한다. 마이그레이션의 원래 목적은 '원할 때 특정 시점이나 상황으로 스키마를 되돌리는 것'이다. rake 명령어는 이를 처리를 위한 다양한 서브 명령어를 제공한다.

  | 명령어 | 설명 | 예시 |
  | :--- | :--- | :--- |
  | db:migrate | 지정한 버전까지 마이그레이션\(Version을 지정하지 않으면 최신으로\) | `rake db:migrate VERSION=20201231022143910` |
  | db:rollback | 지정한 스텝만큼 버전을 되돌림 | `rake db:rollback STEP=5` |
  | db:migrate:redo | 지정한 스텝만큼 버전을 되돌리고 다시 실행 | `rake db:migrate:redo STEP=5` |
  | db:migrate:reset | 데이터베이스를 일단 제거하고 다시 생성한 뒤에 최신 버전의 마이그레이션을 실행 | `rake db:migrate:reset` |

