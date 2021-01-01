# Ruby on Rails 기본 - 컨트롤러

컨트롤러는 비즈니스 로직\(모델\)을 호출하고, 그 결과를 출력\(뷰\)하는 역할을 수행한다.

아래 명령어로 생성하자.

```text
rails generate controller hello
```

책에서는 `app/assets/javascripts/hello.js.coffee`가 생성되는데, rails 6.1.0 버전에서는 생성되지 않는다.

generate한 파일은 아래처럼 destroy 명령어로 일괄삭제할 수 있다.

```text
rails destroy controller hello
```

모든 컨트롤러 클래스는 `ApplicattionController:Base` 클래스를 상속해줘야한다. 이 클래스가 요청이나 응답과 관련된 모든 처리를 해준다. `ApplicationController` 클래스는 `ApplicationController:Base` 클래스를 상속받고 있는 빈 클래스인데 이 클래스를 상속받는게 보통이다.

액션 메서드\(액션\)은 클라이언트로부터의 요청을 처리하는 메서드이다. 컨트롤러 클래스에서 public으로 공개한 메서드를 뜻하며, 컨트롤러 클래스는 한 개 이상의 액션 메서드를 생성할 수 있다.

일반적으로 액션 메서드는 요청을 처리하거나 모델을 호출하고 뷰에서 사용되는 템플릿 변수를 설정하는 등 다양한 일을 처리한다.

원래 MVC는 컨트롤러에서 출력 값을 직접 생성하면 안되지만 디버그 용으로 render 메서드를 자주 쓴다.

```text
render text: value
```

책에서는 route 설정을 route.rb 파일에 아래 URL 패턴으로 match 메서드를 이용해서 우선 설정해준다. Rails가 라우트 관련 이념으로 삼고 있는 RESTful과 맞게 설정하는게 중요하다.

```text
match ':controller(/:action(/:id));, via: [ :get, :post, :patch ]'
```

[Getting started with Rails](https://guides.rubyonrails.org/getting_started.html#mvc-and-you) 에서는 아래처럼 요청 메서드별로 라우팅하였다. [공식 문서](https://api.rubyonrails.org/classes/ActionDispatch/Routing/Mapper/Base.html#method-i-match)에서도 그러라고 되어있다.

```text
get ":controller/:action/:id"
```

**컨트롤러 이름규칙**

| 종류 | 설명 | 예 |
| :--- | :--- | :--- |
| 컨트롤러 클래스 | 앞 글자는 대문자로 뒤에 "Controller"라는 글자를 붙임. | HelloController |
| 컨트롤러 클래스\(파일 이름\) | 컨트롤러 클래스의 이름을 소문자로 만들고, 각 단어를 언더스코어\(\_\)로 구분. | hello\_controller |
| 헬퍼 파일 이름 | 컨트롤러 이름 뒤에 "\_helper.rb"를 붙임. | hello\_helper.rb |
| 테스트 스크립트 이름 | 컨트롤러 이름 뒤에 "\_controller\_test.rb"를 붙임. | hello\_controller\_test.rb |

