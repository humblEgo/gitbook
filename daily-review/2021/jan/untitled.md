# 2021-01-04\(Mon\)



| 항목 | 내용 |
| :--- | :--- |
| 학습 날짜 | 2021-01-04\(월\) |
| 학습 시간 | 12:00~24:00 |
| 학습 범위 및 주제 | 루비온레일즈 라우팅, 테스트 |
| 학습 목표 | 퍼펙트 루비온레일즈 라우팅, 테스트 챕터를 독파한다. |
| 동료 학습 방법 | - |

## 상세 학습 내용

책을 훑으며 아래처럼 간단하게 메모를 진행했다.

## 라우팅

### RESTful 인터페이스

RESTful 인터페이스는 REST의 특징을 가진 라우트를 말한다. REST는 HTTP 메서드와 CRUD를 대응시키는 것이라 생각하면 된다.

* 추출: GET
* 생성: POST
* 변경: PATCH
* 제거: DELETE

Rails는 원칙적으로 RESTful 인터페이스를 기반으로 라우트를 설계한다. 비RESTful적인 라우트를 설정할 수도 있지만, Rails에서 제공하는 `form_for`, `url_for`, `link_to` 등의 뷰 헬퍼는 RESTful 인터페이스를 전제로 설계되어 있으므로 왠만하면 RESTful적인 라우트를 사용하자.

### resources 메서드

RESTful 인터페이스를 정의할 때는 routes.rb에서 [resources 메서드](https://api.rubyonrails.org/classes/ActionDispatch/Routing/Mapper/Resources.html#method-i-resources)를 호출한다.

예를 들어 아래 코드를 적으면 아래 표처럼 URL로 액션이 매핑된다.

```text
Railbook::Application.routes.draw do
  resources :users
  ...
end
```

| URL | 액션 | HTTP 메서드 | 역할 |
| :--- | :--- | :--- | :--- |
| /users\(.:format\) | index | GET | 사용자 목록 생성 |
| /users/:id\(.:format\) | show | GET | 각각의 사용자 상세 화면 생성 |
| /users/new\(.:format\) | new | GET | 신규 사용자 추가 화면 생성 |
| /users\(.:format\) | create | POST | 신규 사용자 화면으로부터의 입력을 받아 등록 처리 |
| /users/:id/edit\(.:format\) | edit | GET | 기존의 사용자 편집 화면 생성 |
| /users/:id\(.:format\) | update | PATCH/PUT | 편집 화면으로부터 입력을 받아 수정 처리 |
| /users/:id\(.:format\) | destory | DELETE | 목록 화면에서 선택한 데이터 제거 처리 |

또한 resources 메서드는 뷰 헬퍼\(link\_to 등\)에서 사용할 수 있는 Url 헬퍼도 자동으로 생성한다.

| 헬퍼 이름\(`_path`\) | 헬퍼 이름\(`_url`\) | 리턴 값\(경로\) |
| :--- | :--- | :--- |
| users\_path | users\_url | /users |
| user\_path\(id\) | user\_url\(id\) | /users/:id |
| new\_user\_path | new\_user\_url | /users/new |
| edit\_user\_path\(id\) | edit\_user\_url\(id\) | /users/:id/edit |

이러한 헬퍼를 사용하면 링크를 보다 직관적으로 사용할 수 있는 것은 물론, 라우트 정의에 의존하지 않을 수도 있다.

### resource 메서드

resources 메서드가 여러 개의 리소스를 관리하는 RESTful 인터페이스를 생성한다면, [resource 메서드](https://api.rubyonrails.org/classes/ActionDispatch/Routing/Mapper/Resources.html#method-i-resource)는 하나의 리소스를 관리하는 RESTful 인터페이스를 생성할 수 있다.

예를 들어 아래 코드를 적으면 아래 표처럼 URL로 액션이 매핑된다.

```text
Railbook::Application.routes.draw do
  resource :config
  ...
end
```

| URL | 액션 | HTTP 메서드 | 역할 |
| :--- | :--- | :--- | :--- |
| /config\(.:format\) | show | GET | 설정 정보 화면 표시 상세 화면 생성 |
| /config/new\(.:format\) | new | GET | 신규 설정 추가 화면 생성 |
| /config\(.:format\) | create | POST | 등록 화면에서 입력을 받아 등록 처리 |
| /config/edit\(.:format\) | edit | GET | 기존 설정 수정 화면 표시 |
| /config\(.:format\) | update | PATCH/PUT | 수정 화면에서 입력을 받아 수정 처리 |
| /config\(.:format\) | destroy | DELETE | 지정된 설정 정보를 제거 처리 |

resources 메서드와 비슷하지만, index 액션이 정의되지 않았으므로 show, edit, delete 등에서 :id를 매개 변수로 받지 않는다는 점이 다르다.

* 참고1\) resource 메서드도 config 리소스가 ConfigsController로 매핑된다.
* 참고2\) `http://localhost:3000/rails/info/routes`로 접근하면 브라우저로도 라우트 정의를 확인할 수 있다.

### RESTful 인터페이스의 사용자 정의화

resources, resource 메서드를 사용하면 RESTful한 정형적인 라우트를 자동으로 생성할 수 있다. 하지만 실제 애플리케이션을 만든다면 표준적인 규칙만으로는 모든 것을 구현할 수 없으므로 중요한 옵션들이 뭐가 있는지는 확인해둘 필요가 있다.

#### 라우트 매개 변수 제약 조건 - contraints 옵션

사실 라우트 매개 변수와 관련된 유효성 검사는 모델에서 하는 것이 기본이지만, 라우트에서 처리하면 원치 않는 값을 보다 확실하게 차단할 수 있다.

`constraints: { <매개 변수 이름>: <정규 표현식> }` 형태로 라우트 매개 변수의 제약 조건을 지정할 수 있다. 아래처럼!

```text
Railbook::Application.routes.draw do
  resources :books, constraints: { id: /[0-9]{1,2}/ }
  ...
end
```

여러 개의 리소스에 동일한 제약 조건을 부여할 때는 다음과 같이 블록 형식으로 제약 조겅늘 작성할 수 있다.

```text
constraints(id: /[0-9]{1,2}/) do
  resources :books
  resources :reviews
end
```

#### 복잡한 제약 조건 설정 - 제약 클래스 정의

정규 표현식만으로는 표현할 수 없는 복잡한 제약 조건을 설정하고 싶다면 제약 클래스를 사용하면 된다. 제약 클래스에 조건을 만들 때는 `matches?` 메서드를 만들어주기만 하면 된다.

`matches?` 메서드는 다음과 같이 규칙을 지켜야한다.

* 매개 변수로 요청 정보\(request 객체\)를 받음.
* 리턴 값으로는 라우트를 유효화할지에 대한 true 또는 false를 리턴

예시를 보자. 아래처럼 제약 클래스를 정의했다면,

```text
class TimeConstaint
  def matches?(request)
    current = Time.now
    current.hour >= 9 && current.hour < 18
  end
end
```

아래처럼 제약 클래스를 불러와서 constraint 옵션으로 전달만 해주면 된다.

```text
require 'TimeConstraint'
​
Railbook::Application.routes.draw.do
  resources :books, constraints: TimeConstraint.new
  ...
end
```

#### form 매개 변수 제거 - form 옵션

resources와 resource 메서드로 정의된 모든 라우트는 `~(.:format)`이 붙어있다. 따라서 `~books.xml` 또는 `~/books.json`처럼 출력 형식을 확장자 형태로 지정할 수도 있다.

하지만 리소스에 따라서는 여러 형식에 대응하지 않을 수도 있다. 이런 경우엔 format 옵션을 false로 지정하자. 그럼 URL 패턴에서 `~(.:format)`이 제거된 라우트가 생성된다.

#### 컨트롤러 클래스와 Url 헬퍼의 이름 수정 - controller와 as 옵션

resources와 resource 메서드는 기본적으로 지정된 리소스 이름을 기반으로 대응되는 컨트롤러를 결정하고 Url 헬퍼를 생성한다.

#### 모듈 내부의 컨트롤러를 맵핑 - namespace와 scope 블록

컨트롤러 클래스의 수가 많아지면 모듈을 사용해 컨트롤러를 폴더로 정리하고 싶은 경우가 있을 것이다. 아래처럼 컨트롤러 클래스를 생성하면 된다.

```text
rails generate controller Admin::Books
```

이렇게 하면 Admin::BooksController가 controllers/admin 폴더 아래에 books\_controller.rb라는 이름으로 생성된다.

이렇게 모듈로 컨트롤러 클래스를 생성할 경우에는 템플릿을 `/views/<모듈 이름>/<컨트롤러 이름>`폴더에 넣으면 된다.

이처럼 모듈을 사용한 컨트롤러 클래스에 RESTful 인터페이스를 정의하려면 아래처럼 namespace 블록을 사용하면 된다. 이렇게 하면 `/admin/books` 또는 `/admin/books/:id`와 같은 URL 패턴과, `admin_books_path` 또는 `admin_book_path(id)`와 같은 URL 헬퍼가 생성된다.

```text
namespace :admin do
  resources :books
end
```

만약에 모듈을 인식만 하고 URL 패턴과 URL 헬퍼에는 영향을 주고 싶지 않은 경우에는 아래처럼 scope 블록을 사용하면 된다.

```text
scope module: :admin do
  resources :books
end
```

#### RESTful 인터페이스에 액션 추가 - collection과 member 블록

collection과 member 블록을 사용하면 resources와 resource 메서드로 자동 생서되는 라우트 외에 원하는 액션을 추가할 수 있다.

```text
resources :name do
  [collection do
    method action
    ...
  end]
  [member do
    method action
    ...
  end]
end
```

collection 블록은 여러 객체를 다루는 액션을, member 블록은 하나의 객체를 다루는 액션을 만들 때 사용한다.

#### RESTful 인터페이스의 액션을 무효화 - only와 except 옵션

collection, member 블록과 반대로 기본적으로 생성되는 액션의 일부를 무효화하고 싶을 때는 only 또는 except 옵션을 지정한다.

라우트 정의를 추가 또는 무효화하는 것이 아니라 표준 액션인 new 액션, edit 액션과 관련된 URL을 변경하고 싶은 경우에는 :path\_names 옵션을 지정한다.

#### 계층 구조를 가진 리소스 표현 - resources 메서드 중첩

리소스들이 애플리케이션 내부에서 계층 관계를 갖는 경우가 있다. 예를 들어, books 리소스\(도서 정보\)는 reviews 리소스\(도서 리뷰\)를 포함한다. 이러한 리소스 계층 관계는 has\_many 또는 belongs\_to 등의 모델 Association으로 나타낸다.

이러한 리소스\(모델\)의 관계는 URL로도 표현해주는 것이 직관적이다. 가령 도서1의 리뷰 정보는 `~/books/1/reviews`처럼 나타내는 것이 좋다.

이러한 관계는 resources와 resource 메서드를 중첩해서 나타낸다.

```text
resources :books do
  resources :reviews
end
```

이러면 URL 헬퍼에 `book_`이라는 접두사가 붙는다. resources와 resource 메서드는 계속해서 중첩할 수 있지만, 계층은 2단계 까지만 만드는 것이 무난하다.

#### 리소스의 얕은 중첩 표현 - shallow 옵션

아래처럼 shallow 옵션을 쓰면 '얕은 URL'이 생성된다.

```text
resources :books do
  resources :reviews, shallow: true
end
```

#### 라우트 정의 재이용 - concern 메서드와 concerns 옵션

concern 메서드를 사용하면 공통되는 내용을 여러 개의 라우트 정의에 넣을 수 있다.

### RESTful 하지 않은 라우트 정의

모든 것을 RESTful하게 만들 수 있는 것은 아니다. 이럴 땐 무리해서 RESTful 인터페이스를 적용하려 들지 말고, 간단하게 RESTful 하지 않은 라우트를 정의해보자.

#### match 메서드

RESTful 하지 않은 라우트를 정의할 때는 [match 메서드](https://api.rubyonrails.org/classes/ActionDispatch/Routing/Mapper/Base.html#method-i-match)를 사용한다.

via 옵션에는 특별한 값으로 :all\(모든 HTTP 메서드 허용\)을 지정할 수도 있다. 하지만 CSRF 공격의 간접적인 원인이 되므로 가급적 사용하지 말자.

#### 다양한 RESTful 하지 않은 라우트 표현

예시를 보자.

```text
get '/blogs/:user_id' => 'blogs#index'
get 'hello/view'
get 'articles(/:category)' => 'articles#index', defaults: { category: 'general', format: 'xml'}
get 'blogs/:user_id' => 'blogs#index', constraints: { user_id: /[A-Za-z]{3, 7}/ }
get ':controller(/:action(/:id))', controller: /common|/[^\/]+/
get 'articles' => 'main#index', as: :top
get 'articles/*category/:id' => 'articles#category'
get '/books/:id' => redirect('/articles/%{id}')
get '/books/:id' => redirect {|p, req| "/articles/#{p[:id].to_i + 10000}"}
```

#### 루트 매핑 정의 - root 메서드

루트에 대응하는 라우트를 설정할 때는 root 메서드를 사용한다. root 메서드는 일반적으로 routes.rb의 마지막에 작성한다.

## 테스트

자동화된 테스트은 안정적이고 유연한 애플리케이션을 만드는데 필수요소다. Rails는 초기 버전부터 이런 자동화된 테스트를 굉장히 중요시하고 있고, 아래 테스트들을 지원한다.

* Unit 테스트: 모델 또는 뷰 헬퍼의 동작 확인
* Functional 테스트: 컨트롤러와 템플릿의 호출 결과 확인\(상태 코드 또는 템플릿 변수 뷰의 출력 결과 등\)
* Integration 테스트: 여러 개의 컨트롤러를 넘나들며 사용자의 실제 조작을 모방하고 결과 확인

테스트 프레임워크로는 RSpec, minitest, Test::Unit 등이 있다. [참고](https://gorails.com/tool_categories/test-frameworks/tools)

책에서는 Test::Unit에 대해 설명한다.

### 테스트 준비

우선 테스트를 하려면 데이터베이스와 테스트 전용 데이터가 필요하다. 예시를 따라가보자.

#### 1. 테스트 데이터 베이스 구축

아래처럼 rake 명령어를 사용해서 테스트 데이터베이스를 구축한다.

```text
rake db:migrate
rake db:test:load
```

`rake db:migrate` 명령어로 미실행 마이그레이션을 실행하고, `schema.rb`를 최신 상태로 업데이트한 뒤에 `rake db:test:load` 명령어로 `schema.rb`를 기반으로 하는 테스트 데이터베이스를 생성한다. 이렇게만 하면 개발 데이터베이스와 같은 구조로 테스트 데이터베이스가 구축된다.

테스트 데이터베이스가 잘 구축되었는지 테스트 환경 데이터베이스로 접속하여 확인할 수 있다. rails 버전별로 명령어가 다르니 주의할 것.

* Rails 3, 4: `rails dbconsole test`
* Rails 5, 6: `rails dbconsole -e test`

  **2. 테스트 데이터 준비**

  픽스처를 `test/fixtures` 디렉토리에 넣어주자.

  **Unit 테스트**

  Unit 테스트\(단위 테스트\)는 애플리케이션을 구성하고 있는 라이브러리\(주요 모델 등\)가 제대로 작동하는지 확인하는 테스트이다.

  **models 테스트**

  `/test/models` 내부에 생성된 스크립트에 [test 메서드](https://api.rubyonrails.org/classes/ActiveSupport/Testing/Declarative.html#method-i-test)를 이용해서 테스트 함수를 만들어주자. 이 때 test메서드도 내부적으로 `test_xxxx` 메서드를 생성하는 것뿐이므로 `test_`로 시작하는 메서드를 직접 정의해도 상관없다.

  이후 `rake test:models` 명령어로 모델 테스트를 실행한다. 만약 특정한 테스트 스크립트를 지정하고 실행하려면 `rake test:models 테스트_스크립트_경로` 를 실행시키면 된다.

  **테스트 메서드가 여러 개 있을 때는 실행 순서가 마음대로이므로 실행 순서에 의존하는 테스트 메서드는 작성하지 않도록 하자.**

  Rails는 테스트를 실행할 때 픽스처를 데이터베이스에 로드하는 것뿐만 아니라, **테스트 스크립트에서 이용할 수 있게 해시로 전개**한다.

  예를 들어 books.yml 내부에 :islb라는 키로 정의된 레코드가 있다면, books\(:jslib\)에 접근할 수 있다.

  **view helper 테스트**

  `/test/helpers` 폴더 내부에 `<컨트롤러>_helper_test.rb`처럼 테스트 스크립트를 만들고 역시 `test`메서드로 테스트를 작성한 다음 `rake test:helpers` 명령어로 실행하자.

  역시나 만약 특정한 테스트 스크립트를 지정하고 실행하려면 `rake test:helpers 테스트_스크립트_경로` 를 실행시키면 된다.

  **테스트 준비와 뒤처리 - setup과 teardown 메서드**

  테스트 스크립트에는 각각의 테스트 메서드가 호출되기 전과 후에 자동으로 호출되는 예약 메서드가 있다. 테스트 스크립트의 부모 클래스 `ActiveSupport::TestCase`에 정의되어 있는 메서드이므로 오버라이드해서 사용한다.

  각각 무슨 역할을 하는지 이름에서 딱 감이 온다.

  * setup: 각 테스트 메서드가 호출되기 전에 실행\(사용할 리소스 초기화\)
  * teardown: 각 테스트 메서드가 호출된 이후에 실행\(사용한 리소스 뒤처리\)

  아래처럼 쓰면 된다.

  ```text
  def setup
    @b = books(:jslib)
  end
  ​
  def teardown
    @b = nil
  end
  ​
  test "where method test" do
    ...
      assert_equal @b.isbn, result.isbn, 'isbn column is wrong'
      assert_equal @b.published, result.published, 'published column is wrong'
  end
  ```

  참고로 데이터베이스 픽스처를 읽는 것과 제거는 Rails가 자동적으로 처리하므로 setup 메서드에서 따로 실행해주지 않아도 된다.

  **Functional 테스트**

  Functional 테스트\(기능 테스트\)는 컨트롤러\(액션\)의 동작 또는 템플릿의 출력을 확인하기 위한 테스트이다. Functional 테스트에서는 브라우저처럼 HTTP 요청을 유사적으로 작성하는 것으로 액션 메서드를 실행하고 그 결과로 HTTP 상태 코드, 템플릿 변수 또는 최종적인 출력 구조 등을 확인한다. 또한, 라우트 정의의 유효성을 확인하는 것도 Functional 테스트의 역할이다.

  **컨트롤러 test 메서드 작성**

  `/test/controllers` 폴더 내부에 `xxx_controller_test.rb`에 test 메서드로 테스트를 작성한다. 아래는 예시.

  ```text
  require "test_helper"
  ​
  class HelloControllerTest < ActionDispatch::IntegrationTest
    test "list action" do
      get :list
      assert_equal 10, assigns(:books).length, 'found rows is wrong.'
      assert_response :success, 'list action failed.'
      assert_template 'hello/list'
    end
  end
  ```

  테스트 메서드를 작성하는 것 자체는 Unit 테스트와 같지만, 몇 가지 Functional 테스트만의 포인트가 있다.

  **get 메서드로 요청 생성**

  Functional 테스트에서는 일단 컨트롤러를 실행하기 위해 [get 메서드](https://api.rubyonrails.org/classes/ActionController/TestCase/Behavior.html#method-i-get)로 HTTP 요청을 생성한다.

  아래는 매개 변수 정보를 함께 전달하는 예제!

  ```text
  get :show, { id: 108 }
  ```

  **Functional 테스트에서 이용할 수 있는 예약 변수**

  Functional 테스트에서는 get, post 등의 메서드를 사용한 후에 아래 예약 변수에 접근할 수 있다.

  | 분류 | 변수 이름 | 설명 |
  | :--- | :--- | :--- |
  | 객체 | @controller | 요청을 처리한 컨트롤러 클래스 |
  | 객체 | @request | 요청 객체 |
  | 객체 | @response | 응답 객체 |
  | 해시 | assigns\(:key\), assigns\['key'\] | 뷰에서 사용할 수 있는 템플릿 변수 |
  | 해시 | cookies\[:key\] | 쿠키 정보 |
  | 해시 | flash\[:key\] | 플래시 정보 |
  | 해시 | session\[:key\] | 세션 정보 |

  **Functional 테스트에서 사용할 수 있는 assert\_xxxx 메서드**

  Unit 테스트에서 썼던 Assertion 메서드 말고도 Functional 테스트에서 추가로 쓸 수 있는 Assertion가 있다. 중요한 것 몇 개를 정리해보자면 아래와 같다.

  1. `assert_difference` 메서드: 처리 후 상태 변화 확인
  2. `assert_generates` 메서드: 라우팅 동작 확인
  3. `assert_select` 메서드: 템플릿 출력 결과 확인

  **Integration 테스트**

  Integration 테스트\(통합 테스트\)는 여러 개의 컨트롤러를 넘나들며 실제 사용자의 행동을 모방할 때 사용한다.

  예를 들어 책에서 구현했던 로그인 기능의 경우 아래 절차를 따른다. Integration 테스트로 이런 단계적인 처리를 실제로 모방해서 각각의 단계에서 제대로 된 처리가 이루어지는지를 확인할 수 있다.

  1. hello\#view 액션에 접근
  2. 미인증 상태이므로, login\#index 액션\(로그인 페이지\)로 리다이렉트
  3. 로그인 페이지에서 사용자 이름과 비밀번호를 입력하고 인증 처리
  4. login\#auth 액션에서 인증 처리가 완료되면 hello\#view 액션으로 리다이렉트

  **테스트 스크립트 예시**

  1. 테스트 스크립트 생성

     Integration 테스트는 직접 `rails generate integration_test xxxxx` 명령어로 생성해줘야한다.

     ```text
     rails generate integration_test admin_login
     # test/integration/admin_login_test.rb 가 생성됨.
     ```

  2. 테스트 스크립트 작성 위에 예시로 적은 로그인 처리를 확인하는 코드 예시는 아래와 같다.

     ```text
     require 'test_helper'
     class AdminLoginTest < ActionDispatch::IntegrationTest
       test "login test" do
         get '/hello/view'
         assert_response :redirect
         assert_redirected_to controller: :login, action: :index
         assert_equal '/hello/view', flash[:referer]
    
         follow_redirect!
         assert_response :success
         assert_equal '/hello/view', flash[:referer]
    
         post '/login/auth', { username: 'arint', password: '12345', referer: '/hello/view' }
         assert_response :redirect
         assert_redirected_to controller: :hello, action: :view
         assert_equal users(:arint).id, session[:usr]
    
         follow_redirect!
         assert_response :success
       end
     end 
     ```

     중간 중간에 있는 `follow_redirect!` 는 응답에 있는 리다이렉트 대상으로 다시 요청하는 기능이다. 미인증 상태의 사용자가 `/hello/view`에 접근하면 로그인 페이지로 리다이렉트하라는 응답을 받게 될텐데, 이 때 받은 리다이렉트 대상으로 이동하는 것을 모방하는 것이다.

  3. 테스트 스크립트 실행

     `rake test:integration` 명령어를 사용하면 테스트를 실행시킬 수 있다.

  \*\*\*\*

## **학습 내용에 대한 개인적인 총평**

RESTful, Test 둘 모두 취업공고를 보면 자주 만날 수 있는 키워드입니다. Rails를 학습하는 과정에서 이 둘을 약간이나마 맛볼 수 있는게 좋네요. Rails를 학습할 때마다 nest.js나 spring 학습에 시간을 쓰고 싶다는 생각이 왕왕들긴 하지만, 이런 식으로 '백엔드'에 필요한 개념과 기술을 확실히 학습해버리는 기회로 삼아야겠습니다.

다음 학습 계획

* 루비온레일즈 클라이언트단 훑기

