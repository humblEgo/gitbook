# SOP와 CORS

## SOP\(Same Origin Policy\)란?

한 origin으로부터 로드된 document 또는 script가 다른 origin의 리소스와 상호작용 할 수 있는 방법을 제한하는 중요한 보안 메커니즘이다.

* 브라우저가 document를 받을 때는 그 document의 출처를 origin이라는 이름으로 저장해둔다.
* 그런데 이 때 document 내에서 외부리소스들과 상호작용 할 때 최초 파악한 origin과 다르다면, 별도의 제한을 두게 되는 것이다.
* 이 origin을 파악할 때는 scheme, Host, Port만 보고서 판단하게 된다. `https://www.example.com:443/path/page.html` 이 origin이라고 치면, scheme\(`https`\)과 Host\(`www.example.com`\)와 port\(`443`\)이 동일한 URL을 same origin으로 판별하게 되는 것이다. 이 때 port는 생략되어있을 때 scheme에 할당되는 포트번호를 기본으로 판별하게 된다.

### 왜 SOP가 중요할까?

해커가 악의적인 코드를 넣은 사이트를 유저가 열어버린 시나리오를 생각해보자.

1. 만약 유저가 gmail 등에 접속해둔 상태라면 쿠키가 살아있을 것이고, 해커는 '악의적인 코드'를 통해 이 쿠키를 자동으로 첨부하여 구글 메일 서버에 유저의 메일 정보를 요청할 수 있다.
2. 구글 메일 서버는 쿠키만 확인하고 메일 정보를 응답한다.
3. 이제 SOP 유무에 따라 해커에게 메일정보가 넘어가는지 유무가 결정된다.
   * SOP가 없다면? 메일 정보가 그대로 해커에게 넘어간다.
   * SOP가 있다면? origin이 gmail이 아니라 해커의 사이트이므로 CORS 정책에 의해 메일 정보 read가 불가하다. 해커는 정보를 읽을 수 없다!

### Cross-origin Network Access를 허용하는 방법

Cross-origin간의 통신이 필요한 경우는?

* JSONP
  * NO!
  * 모든 origin 대상으로 SOP를 무력화
* CORS
  * YES!
  * 허용할 origin만을 `Access-Control-Allow-Origin`에 명시적으로 추가

