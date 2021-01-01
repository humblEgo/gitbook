# 뷰 개발

책 기준으로는.. 입력 양식\(&lt;form&gt; 태그\)을 생성하기 위한 헬퍼는 2종류가 있었다.

1. `form_for` : 특정 모델 객체를 처리할 때 특화된 메서드
2. `form_tag`: 모델과 관계 없는, 범용적인 입력 양식을 생성할 때 사용하는 메서드

그런데 찾아보니 **이제 둘 다 거의 쓰이지 않고, `form_with`를 써야한다고 한다.** [form\_for vs form\_with](https://stackoverflow.com/questions/43868976/rails-5-form-for-vs-form-with)를 참고하자. [관련한 PR](https://github.com/rails/rails/pull/26976)에 이유가 적혀있다.

흠 뷰 헬퍼들이 뭐가 있는지만 파악하고, 자세한 사용은 API 문서보면서 바로바로 적용하면 무방하겠다.

* `link_to_unless_current` : 링크 대상이 현재 페이지일 경우에 링크로 이동하는 대신 문자만 출력한다. 레이아웃 위에 공통 메뉴 등을 생성할 때 편리한 메서드이다.

### 사용자 정의 뷰 헬퍼

Rails 4는 기본적으로 `/app/helpers` 폴더 내부에 있는 모든 `xxx_helper.rb` 모듈을 읽어 들이도록 설정되어ㅣ있다. 모듈간에도 메서드를 공통적으로 활용하기 좋지만 메서드 이름이 중복되면 메서드를 구분할 수 없게 된다.

이 경우 아래처럼 `application.rb`에 `config.action_controller.include_all_helpers = false`로 설정해주면 된다.

```text
class Application < Rails::Application
  config.action_controller.include_all_helpers = false
  ...
end
```

이렇게 설정해주면 `application_helper.rb`와 현재 컨트롤러에 대응되는 `<컨트롤러 이름>_helper.rb`만 읽어 들인다. 애플리케이션 공통 헬퍼는 전자에, 특정 컨트롤러만을 위한 헬퍼는 후자에 작성하면 된다.

### 레이아웃

Rails는 레이아웃을 따로 지정하지 않은 경우, `/app/views/layouts` 폴더의 `application.html.erb`를 레이아웃으로 적용한다. 일반적으로 이 파일을 수정해서 개발한다.

하지만 상황에 따라 컨트롤러 또는 액션 단위로 레이아웃을 변경하거나 적용하지 않을 수 있다.

1. 컨트롤러 단위로 레이아웃을 설정 `/app/views/layouts` 폴더 내부에 `<컨트롤러 이름>.html.erb`라는 이름으로 저장.
2. 컨트롤러 단위로 레이아웃을 설정\(layout 메서드\) layout 메소드로 컨트롤러 클래스 내부에 적용할 레이아웃을 명시적으로 지정.
3. 액션 단위로 레이아웃 설정 render 메서드에 layout 옵션을 지정.
4. 페이지 단위로 타이틀을 변경 일반적인 템플릿 파일과 마찬가지로 템플릿 변수를 전달할 수 있다. `layouts/applicaiton.html.erb`에 템플릿 변수로 타이틀 변경하게 만든 예시

   ```text
   <!DOCTYPE html>
   <html>
   <head>
     <title><%= @title ? @title : 'Rails 입문' %></title>
   ```

### 부분 템플릿

부분 템플릿을 저장할 때는 파일 이름 앞에 "\_"를 붙여야한다. 이러한 규칙을 지킨다면 `/app/views` 폴더 아래에 아무곳에나 놔도 Rails가 알아서 인식하긴한다.

하지만 관리를 위해 아래 규칙을 지켜서 위치시키자.

| 용도 | 저장 폴더 |
| :--- | :--- |
| 특정 컨트롤러에서만 공유 | /views/&lt;컨트롤러 이름&gt; |
| 애플리케이션 전체에서 공유 | /views/application |
| 리소스와 연관되어 있는 부품 | /views/&lt;리소스 이름\(복수형\)&gt; |

