---
description: >-
  모든 것을 RESTful하게 만들 수 있는 것은 아니다. 이럴 땐 무리해서 RESTful 인터페이스를 적용하려 들지 말고, 간단하게
  RESTful 하지 않은 라우트를 정의해보자.
---

# 라우팅 - RESTful 하지 않은 라우트 정의

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

