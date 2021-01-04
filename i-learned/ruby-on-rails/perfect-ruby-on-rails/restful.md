# 라우팅 - RESTful

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

