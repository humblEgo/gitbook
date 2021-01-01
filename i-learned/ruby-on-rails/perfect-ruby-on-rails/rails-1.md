# 스캐폴딩 기능을 사용한 Rails 개발 기초

스캐폴딩으로 빠르게 CRUD 기능을 갖춘 컨트롤러 클래스를 사용할 수 있다.스캐폴딩 기능을 사용할 때도 rails genrate 명령어를 사용한다.

```text
rails generate scaffold name field:type [...] [option]
```

한방에 해결! 단, 마이그레이션으로 데이터를 생성하는 것만은 rake 명령어로 별도로 실행해줘야한다.

```text
rake db:migrate
```

스캐폴딩으로 애플리케이션을 생성하면 `config/routes.rb`에 resources 메서드가 추가된다.

책에서는 `rake routes` 로 라우팅을 확인하는데 `rails routes` 명령어로 라우팅을 확인할 수 있다. 바뀐 듯.

URL 요청에 아래처럼 `.json` 을 요청할 수도 있는데, 이 때 JSON 형식으로 출력할 때 사용하는 템플릿은 `<컨트롤러 이름>/<액션이름>.json.builder` 이름으로 `app/views` 폴더에 생성된다.

#### 뷰 헬퍼 사용하기

뷰 헬퍼\(View Helper\)는 템플리 파일을 작성할 때 도움을 주는 메서드다.

뷰 헬퍼 중 `link_to` 메서드의 url로 모델 객체를 전달하면 Rails는 해당 객체의 id 속성을 이용해서 링크를 만든다.

```text
<%= link_to 'Show', book %> #/books/1 형태의 경로 생성됨.
```

예제에서는 라우터 정의에 따라 아래처럼 뷰 헬퍼가 자동으로 정의되었다.

| 헬퍼 이름 | 경로 |
| :--- | :--- |
| book\_path | /books |
| book\_path\(id\) | /books/:id |
| new\_book\_path | /books/new |
| edit\_book\_path\(id\) | /books/:id/edit |

#### 상세페이지 만들기

`new.html.erb` 에서는 아래처럼 'form'과 'Back' 템플릿을 불러와서 화면을 구성한다.

```text
<h1>New Book</h1>
​
<%= render 'form', book: @book %>
​
<%= link_to 'Back', books_path %>
```

이 때 불러와지는 템플릿을 부분 템플릿이라고 부르며, 부분 템플릿을 출력할 때는 render 메서드를 활용한다.

부분 템플릿은 `_<이름>.html.erb` 형태로 생성해야한다.

