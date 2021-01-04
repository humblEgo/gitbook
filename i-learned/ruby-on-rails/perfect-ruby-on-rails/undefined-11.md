---
description: 우선 테스트를 하려면 데이터베이스와 테스트 전용 데이터가 필요하다. 예시를 따라가보자.
---

# 테스트 - 준비

#### 1. 테스트 데이터 베이스 구축

아래처럼 rake 명령어를 사용해서 테스트 데이터베이스를 구축한다.

```text
rake db:migrate
rake db:test:load
```

`rake db:migrate` 명령어로 미실행 마이그레이션을 실행하고, `schema.rb`를 최신 상태로 업데이트한 뒤에 `rake db:test:load` 명령어로 `schema.rb`를 기반으로 하는 테스트 데이터베이스를 생성한다. 이렇게만 하면 개발 데이터베이스와 같은 구조로 테스트 데이터베이스가 구축된다.

테스트 데이터베이스가 잘 구축되었는지 테스트 환경 데이터베이스로 접속하여 확인할 수 있다. rails 버전별로 명령어가 다르니 주의할 것.

* Rails 3, 4: `rails dbconsole test`
* Rails 5, 6: `rails dbconsole -e test`

**2. 테스트 데이터 준비**

픽스처를 `test/fixtures` 디렉토리에 넣어주자.

