# 클라이언트 개발 - Sprockets vs Webpacker

우선 에셋 개념을 짚자. 애셋\(Asset\)이란 자바스크립트 코드, 스타일시트, 그림처럼 애플리케이션에서 다루는 모든 리소스의 총칭이다.

Rails4 에서는 에셋을 읽어들이기 위해 Sprockets 라는 라이브러리를 사용한다. Rails6 에서는 Sprockets 말고도 Webpacker라는 라이브러리를 함께 사용한다. [같이 사용하는 이유](https://rossta.net/blog/why-does-rails-install-both-webpacker-and-sprockets.html)를 찾아보니 Webpacker는 자바스크립트 소스파일을 다루는데 쓰고, Sprockets는 나머지 전부를 다루는데 쓰는 것 같다.

> "Rails way": Webpacker to compile JavaScript, Sprockets for CSS, images, and fonts

어차피 두 라이브러리는 서로 의존성을 1도 가지고 있지 않다. 따라서 Sprockets만 쓰거나 Webpacker만 쓰는 것도 유효하다. 즉, 둘 다 선택지로 존재한다는 소리고 상황에 따라 선택해야할 문제다. 아래 예시를 참고하자.

**Sprockets을 쓸 이유\(상황\)**

* 내 Rails 앱이 자바스크립트를 많이 안 쓴다.
* 전역 스크립트와 jQuery를 선호한다. 예를 들어 자바스크립트 모듈 시스템을 쓸 필요가 없는 상황이다.
* legacy Rails 앱을 Webpacker로 업그레이드 비용이 넘 비싸다.
* 로컬 개발을 위해 고급 툴링\(tooling\)을 할 필요가 없다.
* 걍 돌아가는 서비스를 만들기를 원한다. 막 좋은 대안을 찾을 시간이 없다.
* 내 Rails 앱이 특정 gem을 사용하며, NPM을 대신 할 수 없다.

**Sprockets를 쓰지 않을 이유\(상황\)**

* Sprockets는 로컬개발환경에서 느리다.
* asset을 좀 더 섬세하게 제어해야한다.
* 내 앱에는 JavaScript가 많고 대량의 페이로드를 방지하기 위해 코드 분할 기능이 필요하다.
* 장기적으로 지원을 유지할 서비스다.

#### Webpacker를 쓸 이유\(상황\)

* 의존성 관리하는데 자바스크립트 모듈 시스템을 쓰는 것을 선호한다. 예를 들어 전역 스코프가 오염되었거나 명시적인 종속성 그래프가 요구된다.
* ES6+, Babel, PostCS의 최첨단 기능을 활용하고 싶다.
* 동적 가져오기 및 웹 팩의 splitChunks 최적화와 같은 지능형 코드 분할 기능을 원한다.
* 빌드 시스템에서 소스 맵을 생성하는 방식으로 더 많은 유연성을 원한다.
* hot module replacement 같은 고급 툴링\(tooling\)이 필요하다.
* 싱글 페이지 앱을 만들고 싶다. 아, 물론 Webpack을 쓰려고 싱글 페이지 앱을 만들 필요는 없다. Webpack은 멀티 페이지 앱에서 더 잘 작동한다.

#### Webpacker를 쓰지 않을 이유\(상황\)

* 내 Rails 앱이 자바스크립트를 많이 안쓴다.
* 자바스크립트 에코시스템에 대해서 지식이 부족하다.
* webpack과 Webpacker 학습에 시간을 쓸 수가 없다.
* 넘 복잡해보인다.

### Sprockets로 자바스크립트, 스타일시트 임포트

Sprockets는 에셋을 매니페스트, 즉 미리 준비된 리소스로 읽어들인다. `app/assets` 폴더를 확인하자.

매니페스트 정의는 각각 주석\(//, /\* ~ \*/\)의 내부에 `=<지시자> <매개 변수>`의 현식으로 표기한다.

참고로 Rails는 클라이언트 사이드 개발 언어로 커피스크립트와 SCSS를 표준으로 지원하고 있다. 하지만 반드시 사용할 필요는 없다.

