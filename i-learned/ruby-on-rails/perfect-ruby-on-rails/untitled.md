---
description: 액션에서의 처리 결과를 출력하기 위한 메서드들이 있다. 편의상 응답 메서드라고 부르며 메모하자.
---

# 컨트롤러 개발 - 응답

| 응답 메서드 | 설명 |
| :--- | :--- |
| render | 템플릿 호출 또는 글자/스크립트 출력 등, 범용적인 출력 |
| redirect\_to | 지정된 주소로 처리를 리다이렉트 |
| send\_file | 지정된 파일을 출력 |
| send\_data | 지정된 바이너리 데이터를 출력 |

## render 메서드

액션에서 명시적으로 응답 메서드를 호출하지 않으면 자동으로 render 메서드가 호출되어 템플릿 파일을 실행하게 된다. render 메서드는 템플릿을 호출하거나 응답을 인라인으로 설정하는 등 다양한 옵션을 가지고 있다. 후자의 경우 MVC 관점에서 어긋나니 주의하자. 디버그용으로만 쓸 것.

{% hint style="info" %}
뷰에 yield 구가 하나만 있는데 render를 여러번 호출하면 에러가 발생하니 주의하자.
{% endhint %}

{% hint style="info" %}
render를 head 메서드처럼 응답 상태값을 받는 용도로도 쓸 수 있다.
{% endhint %}

## redirect\_to 메서드

`redirect_to` 메서드는 지정된 페이지로 처리를 리다이렉트시킨다.

```text
reirect_to url [, status=302]
​
# urls: 리다이렉트 대상 URL
# status: 상태 코드(숫자 또는 심볼)
```

메서드의 인자로 들어가는 url은 문자열 또는 해시 형식으로 지정하면 된다. 예시를 보자.

```text
redirect_to 'http://www.wings.msn.to'           #절대 경로
redirect_to action: :index                      #같은 컨트롤러의 액션
redirect_to controller: :hello, action: :list   #다른 컨트롤러의 액션
redirect_to books_path                          #자동 생성된 뷰 헬퍼
redirect_to :back                               #이전 페이지
```

## send\_file 메서드

지정한 경로에 있는 파일을 읽어 들여 그 내용을 클라이언트에게 전송한다.

```text
send_file path [, opts]
# path: 읽어 들일 파일의 경로
# opts: 동작 옵션
```

{% hint style="info" %}
요청 정보로 파일 경로를 직접 지정하면 사용자가 서버 내부의 파일에 접근할 수도 있게되므로 굉장히 위험하다. 아래 같은 코드는 피하자.

`send_file params[:path]`
{% endhint %}

## send\_data 메서드

매개 변수로 바이너리 데이터를 받고 응답한다.

```text
send_data data [,opts]
# data: 파일 경로
# opts: 동작 옵션
```

데이터베이스에서 바이너리 자료형을 추출하고 클라이언트에게 응답하는 경우 레코드를 추출하고, 콘텐츠 타입을 나타내는 필드\(ctype 필드\)를 매개 변수 type에, 바이너리 필드를 매개 변수 data에 전달해주면 된다.

## logger 객체

logger 객체는 로그를 사용하기 위해 Rails가 제공하는 객체이다. 로그의 중요도에 따라 6개의 메서드를 제공하고 있다. 아래는 우선순위가 높은 순에서 낮은 순으로 정리한 메서드.

1. `unknown(msg)`: 알 수 없는 오류
2. `fatal(msg)`: 치명적인 오류
3. `error(msg)`: 오류
4. `warn(msg)`: 경고
5. `info(msg)`: 정보
6. `debug(msg)`: 디버그 정보

로그 출력 레벨을 변경하려면 `development.rb` 에 `config.log_level = :error` 같은 내용을 추가하면 된다. 또한 `filter_parameter_logging.rb` 를 수정하여 패스워드 등의 파일을 로그에 기록되지 않도록 설정할 수도 있다.

## HTML 이외의 응답 처리

추출한 모델의 내용을 XML 또는 JSON 형식으로 출력하는 것은 굉장히 간단하다.

render 메서드에 xml 또는 json 옵션을 지정하기만 하면 된다. 가령 xml 옵션을 지정하고 일반적인 모델을 넣어주면 아래 처리를 자동으로 수행한다.

* `to_xml` 메서드로 모델을 XML 형식으로 변환
* Content-Type 헤더를 "application/xml"로 지정

그러나 이처럼 render 메서드에 json과 xml 옵션을 지정해서 JSON 또는 XML 형식의 응답을 생성하는 것은 편리하지만, 결과 생성은 뷰에서 처리한다는 MVC의 기본 정책에는 위반된다. 또한, json과 xml 옵션으로 모델을 출력하는 방식은 모델의 내용을 기계적으로 변환하는 것뿐이므로 원하는 형식을 만들어 내는 데는 문제가 있다.

## 템플릿으로 JSON과 XML 데이터 생성 - Jbuilder/Builder

ERB로 HTML 데이터를 생성하는 것처럼 템플릿을 기반으로 하여 JSON과 XML 데이터를 생성하는 것이 바람직하다. 이를 수애하는 것이 바로 JBuilder와 Builder 템플릿이다. 각각 JSON 데이터를 생성하는데, XML 데이터를 생성하는 데에 특화된 템플릿이다.

한편 Builder에 있는 뷰 헬퍼인 atom\_feed 메서드를 사용하면 Atom 피드를 쉽게 생성할 수 있다.

## 멀티 포맷으로 출력 - response\_to 메서드

Rails에서 멀티 포맷을 사용할 때는 ERB, Jbuilder, Builder, Ruby 등의 템프릿을 사용해 원하는 뷰를 여러 개 준비해두는 것이 기본이다.

하지만 아래 경우에는 `respond_to` 메서드를 이용해 간단하게 분기 처리를 할 수 있다.

* 디버그 전용 오류 글자를 출력하고 싶은 경우
* 템플릿을 준비할 것도 없는 경우
* 각각의 형식에 따라 리다이렉트를 하고 싶은 경우
* 헤더만 출력하고 싶은 경우

```text
respond_to do |format|
  foramt.type { statements }
  ...
end
```

`respond_to` 메서드의 내부 블록에는 format.type 형식으로 원하는 형식 type을 적는다. 이후 statements 부분에는 형식에 따라 원하는 처리 코드를 입력한다.

이 때 `respond_to` 메서드에서 사용할 수 있는 형식은 Rails의 `actions_dispatch/http/mime_types.rb`에 정의되어 있다. 만약 기존에 정의된 형식 외에 다른 형식을 사용하고 싶은 경우에는 `/config/initializers/mime_types.rb`에서 아래 형식으로 포맷을 등록하면 된다.

```text
Mime::Type.register "text/richtext", :rtf
Mime::Type.register_alias "text/html", :iphone
```



