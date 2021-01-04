# 테스트 - Functional 테스트

Functional 테스트\(기능 테스트\)는 컨트롤러\(액션\)의 동작 또는 템플릿의 출력을 확인하기 위한 테스트이다. Functional 테스트에서는 브라우저처럼 HTTP 요청을 유사적으로 작성하는 것으로 액션 메서드를 실행하고 그 결과로 HTTP 상태 코드, 템플릿 변수 또는 최종적인 출력 구조 등을 확인한다. 또한, 라우트 정의의 유효성을 확인하는 것도 Functional 테스트의 역할이다.

#### 컨트롤러 test 메서드 작성

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

