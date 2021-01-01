# 모델 개발 - 콜백



콜백은 액티브 레코드로 검색, 등록, 변경, 제거 또는 유효성 검사 시점에서 실행되는 메서드를 의미한다. 예를 들어 다음 사항들처럼 모델을 조작할 때 함께 실행할 처리를 콜백으로 정의하여 코드가 모델 또는 컨트롤러에 분산되는 것을 막을 수 있다.

* 사용자 정보를 등록할 때 비밀번호가 지정되지 않았다면, 임의의 비밀번호를 생성
* 도서 정보를 제거한 뒤, 삭제 이력을 저장
* 저자 정보를 제거할 때 파일 시스템으로 관리하고 있는 섬네일 이미지도 함께 제거
* 저자 정보가 등록 또는 변경될 때 관리자에게 메일 전송

또한, 액티브 레코드는 실제 저장 처리와 콜백을 하나의 트랜잭션으로 실행한다. 따라서 콜백을 사용하면 관련된 처리를 한꺼번에 실행할 때 트랜잭션과 관련된 내요은 신경 쓰지 않아도 된다.

신규 등록/변경/제거 시점에 실행되는 콜백을 발생 순서대로 적은 것은 아래와 같다.

* `before_validation`
* `after_validation`
* `before_save`
* `around_save`
* `before_create`
* `around_create`
  * `after_create`
  * `after_save`

    데이터 추출이나 객체 생성 시점에 호출되는 콜백도 있다.

    * `after_find` : 데이터베이스 검색 시점에 실행된다.
    * `after_initialize` : new로 생성, 데이터베이스에서 로드하는 시점에 실행된다.

    `after_destroy` 콜백을 사용하는 구문을 보자.

    ```text
    class Book < ActiveRecord::Base
      after_destroy :history_book
  
      private
      def history_book
        logger.info('deleted: ' + self.inspect)
      end
    end
    ```

    `after_destory` 메서드로 콜백 메서드 `history_book`을 등록하고, `history_book` 메서드는 private 메서드로 선언했다.

    만약 조건을 가진 콜백을 적요앟려면 아래처럼 if나 unless 매개 변수를 지정하자.

    ```text
    after_destroy :histroy book,
      unless: Proc.new { |b| b.publish == "unknown" }
    ```

    콜백은 간단하게 블록으로 지정하는 것도 가능하지만, 여러 모델에서 적용해야하면 콜백 메서드를 별도의 클래스로 외부 위치에 정의하고 재사용하는 것이 좋다.

