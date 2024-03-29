# Rails lib 디렉토리에 대하여

### 용도

관례상 아래 용도로 쓰인다.

1. 모델, 뷰, 컨트롤러에 해당되지 않는 애플리케이션 코드를 위치시킨다.
2. 모델, 뷰, 컨트롤러 사이에서 공유되는 코드를 두는 곳으로도 쓰인다.

### 참조 방법

* 레일즈 1, 2: lib 디렉터리에 있는 파일들이 require 문들을 해석하는 데 쓰이는 불러오기 경로에 자동으로 포함됨.
* 레일즈 3 이후 : 옵션을 따로 켜줘야 require 문들을 해석하는 데 쓰이는 불러오기 경로에 포함됨.
  * `config/application.rb` 에 `config.autoload_paths += %W(#{Rails.root}/lib)` 내용 추가 필요.

#### 자동 불러오기?

한편, 클래스나 모듈을 담고 있는 파일이 해당 클래스나 모듈의 이름을 소문자로 바꿔 자신의 이름으로 쓰고 있다면, 레일즈는 그 파일을 자동으로 불러온다.

ex\) `lib/pdf_stuff/receipt.rb`파일 안에 클래스 이름을 `PdfStuff::Receipt` 라고 짓기만 하면, 레일즈가 이를 자동으로 찾아서 불러올 것이다. 이런 자동 불러오기 조건을 만족하지 못할 때 require 메커니즘을 쓰자.

