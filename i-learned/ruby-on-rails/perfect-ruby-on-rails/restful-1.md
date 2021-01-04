---
description: >-
  resources, resource 메서드를 사용하면 RESTful한 정형적인 라우트를 자동으로 생성할 수 있다. 하지만 실제 애플리케이션을
  만든다면 표준적인 규칙만으로는 모든 것을 구현할 수 없으므로 중요한 옵션들이 뭐가 있는지는 확인해둘 필요가 있다.
---

# 라우팅 - RESTful 인터페이스의 사용자 정의화

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

