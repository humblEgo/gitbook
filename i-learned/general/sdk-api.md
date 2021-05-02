---
description: >-
  PG 연동 개발자들에게 쉽고 빠른 가이드 제공을 위해, 20년 동안 유지된 기존 시스템에 추상화 레이어를 만들고 SDK와 API를 설계했던
  경험을 공유합니다. 추가로 인터페이스의 일관성과 예측 가능성 그리고 TypeScript와 npm 모듈 지원을 위한 시도들까지, 토스페이먼츠
  오픈 API의 첫 여정에 대해 공유합니다. - Slash21 by TOSS
---

# 결제 시스템의 SDK와 API 디자인 - SLASH 21

### 가맹점에 API를 제공하기 위한 3대 원칙을 정했다.

1. 고객\(가맹점\) 편의가 우선
2. 쉽고 간결한 디자인
3. 웹 표준 사용

RESTful Design은 API 인터페이스를 이해하기 쉽게 해주기 때문에 도입하고 싶었으나, 실제 도입 시에는 몇 가지 문제점이 있었다.

### 고객\(가맹점\) 편의를 우선하기 위해 'Not Pure RESTful API'를 설계하게 되었다.

* Pure RESTful API라면 CRUD가 HTTP 메소드와 직관적으로 맵핑된다.
* 하지만 많은 가맹점이 DELETE, PUT 사용이 불가한 경우가 많았다.
  * HTTP 클라이언트 모듈이 DELETE, PUT 미지원
  * 가맹점 인프라, 방화벽 문제로 DELETE, PUT 불가

이에 따라 DELETE, PUT을 배제하고 GET과 POST만으로 CRUD가 가능하게 API를 설계하였다.

### 이 때 정한 API 디자인 가이드

* GET : 리소스의 상태 변화를 주지 않는 동작에 연결
* POST : 리소스의 상태 변화를 주는 동작에 연결

특정 도메인에 대해 두 개 이상의 상태 변화시킬 일이 있다면 API 규칙이 충돌할 수 있다. 가령 결제 승인과 취소를 할 경우, 둘 다 `/payments/{key}`에 POST를 날리게 되는 것이다.

이런 충돌은 명사형 표현 하위에 동사형 단어를 넣는 기준을 마련하여 해결하였다.

```text
POST /payments/{key}        // 결제 승인
POST /payments/{key}/cancel // 결제 취소
```

예측 가능한 일관성을 통해 고객의 편의를 지키는 것이 우선이었다. **요는 RESTful 디자인을 고객 편의를 해치지 않는 선에서 적용한 것!**

![image](https://user-images.githubusercontent.com/54612343/116695246-484fb200-a9fb-11eb-8e90-36a53964fd92.png)

한편 JSON은 계층 표현이 가능하기 때문에 Nested Object를 모듈화하여 표현하였다. **특히 다른 응답에서 반복적으로 사용될 수 있는 정보는 모듈화했다.**

![&#xC2A4;&#xD06C;&#xB9B0;&#xC0F7; 2021-04-30 &#xC624;&#xD6C4; 9 31 07](https://user-images.githubusercontent.com/54612343/116698345-396afe80-a9ff-11eb-8b03-9916db9c81cd.png)

이를 통한 이점은 아래와 같다.

1. 모듈화된 객체가 유효한지에 따라 null로 보내서 체크하기 쉽게 할 수 있다.
2. 중복되는 표현을 피할 수도 있다.
3. 반복될 수 있는 필드들을 하나의 필드로 묶어서 예상하기 쉽게 만든다는 장점이 있다.

![&#xC2A4;&#xD06C;&#xB9B0;&#xC0F7; 2021-04-30 &#xC624;&#xD6C4; 9 34 30](https://user-images.githubusercontent.com/54612343/116696599-150e2280-a9fd-11eb-8ccc-b10b96db7242.png)

Enumeration, Type을 한글로 표현해서 가독성을 올렸다. 한편 한글을 쓰지 않는 고객사를 고려하여, Accept-Language 헤더에 따라 응답값 언어가 달라지도록 하였다.

![&#xC2A4;&#xD06C;&#xB9B0;&#xC0F7; 2021-04-30 &#xC624;&#xD6C4; 9 36 35](https://user-images.githubusercontent.com/54612343/116696637-22c3a800-a9fd-11eb-8ecb-0c2b6597de8a.png)

그리고 에러 상태값과 Response body에 오류정보를 넣어서 고객사가 오류 정보를 쉽게 확인할 수 있도록 하고, code를 넣어서 고객사가 원한다면 쉽게 에러 메세지를 바꿀 수 있도록 하였다.

![image](https://user-images.githubusercontent.com/54612343/116696041-56520280-a9fc-11eb-8a8e-cc545fb791cf.png)

자체적인 암호화, 기타 표준을 사용하면 아래 문제에 노출된다.

* 연동 난이도가 높아지는 문제
* 자체적인 취약점을 가질 수 있다는 문제
* 실수할 가능성이 높아지는 문제

웹 표준을 채택하여 위 문제들을 회피하였다.

그리고 웹 표준만을 사용했을 때의 보안을 높이기 위해 아래 구조를 만들었다.

![image](https://user-images.githubusercontent.com/54612343/116696215-931df980-a9fc-11eb-9a3c-3f539a5d907d.png)

결제가 성공하려면 마지막에 항상 가맹점 서버의 API 호출이 있어야만 한다. 여기서도 도중에 위변조나 해킹의 위험을 고려해서 FDS 적용해서 정보의 위변조 여부를 검증한다.

* 영상링크:[htps://www.youtube.com/watch?time\_continue=964&v=E4\_0WWqmF3M&feature=emb\_logo](https://www.youtube.com/watch?time_continue=964&v=E4_0WWqmF3M&feature=emb_logo)

