# 테스트

자동화된 테스트은 안정적이고 유연한 애플리케이션을 만드는데 필수요소다. Rails는 초기 버전부터 이런 자동화된 테스트를 굉장히 중요시하고 있고, 아래 테스트들을 지원한다.

* Unit 테스트: 모델 또는 뷰 헬퍼의 동작 확인
* Functional 테스트: 컨트롤러와 템플릿의 호출 결과 확인\(상태 코드 또는 템플릿 변수 뷰의 출력 결과 등\)
* Integration 테스트: 여러 개의 컨트롤러를 넘나들며 사용자의 실제 조작을 모방하고 결과 확인

테스트 프레임워크로는 RSpec, minitest, Test::Unit 등이 있다. [참고](https://gorails.com/tool_categories/test-frameworks/tools)

책에서는 Test::Unit에 대해 설명한다.

