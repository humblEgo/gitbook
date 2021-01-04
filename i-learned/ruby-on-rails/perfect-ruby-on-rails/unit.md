# 테스트 - Unit 테스트

Unit 테스트\(단위 테스트\)는 애플리케이션을 구성하고 있는 라이브러리\(주요 모델 등\)가 제대로 작동하는지 확인하는 테스트이다.

#### models 테스트

`/test/models` 내부에 생성된 스크립트에 [test 메서드](https://api.rubyonrails.org/classes/ActiveSupport/Testing/Declarative.html#method-i-test)를 이용해서 테스트 함수를 만들어주자. 이 때 test메서드도 내부적으로 `test_xxxx` 메서드를 생성하는 것뿐이므로 `test_`로 시작하는 메서드를 직접 정의해도 상관없다.

이후 `rake test:models` 명령어로 모델 테스트를 실행한다. 만약 특정한 테스트 스크립트를 지정하고 실행하려면 `rake test:models 테스트_스크립트_경로` 를 실행시키면 된다.

**테스트 메서드가 여러 개 있을 때는 실행 순서가 마음대로이므로 실행 순서에 의존하는 테스트 메서드는 작성하지 않도록 하자.**

Rails는 테스트를 실행할 때 픽스처를 데이터베이스에 로드하는 것뿐만 아니라, **테스트 스크립트에서 이용할 수 있게 해시로 전개**한다.

예를 들어 books.yml 내부에 :islb라는 키로 정의된 레코드가 있다면, books\(:jslib\)에 접근할 수 있다.

#### view helper 테스트

`/test/helpers` 폴더 내부에 `<컨트롤러>_helper_test.rb`처럼 테스트 스크립트를 만들고 역시 `test`메서드로 테스트를 작성한 다음 `rake test:helpers` 명령어로 실행하자.

역시나 만약 특정한 테스트 스크립트를 지정하고 실행하려면 `rake test:helpers 테스트_스크립트_경로` 를 실행시키면 된다.

#### 테스트 준비와 뒤처리 - setup과 teardown 메서드

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

