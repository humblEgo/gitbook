# 테스트 - Integration 테스트



Integration 테스트\(통합 테스트\)는 여러 개의 컨트롤러를 넘나들며 실제 사용자의 행동을 모방할 때 사용한다.

예를 들어 책에서 구현했던 로그인 기능의 경우 아래 절차를 따른다. Integration 테스트로 이런 단계적인 처리를 실제로 모방해서 각각의 단계에서 제대로 된 처리가 이루어지는지를 확인할 수 있다.

1. hello\#view 액션에 접근
2. 미인증 상태이므로, login\#index 액션\(로그인 페이지\)로 리다이렉트
3. 로그인 페이지에서 사용자 이름과 비밀번호를 입력하고 인증 처리
4. login\#auth 액션에서 인증 처리가 완료되면 hello\#view 액션으로 리다이렉트

### 테스트 스크립트 예시

1. 테스트 스크립트 생성

   Integration 테스트는 직접 `rails generate integration_test xxxxx` 명령어로 생성해줘야한다.

   ```text
   rails generate integration_test admin_login
   # test/integration/admin_login_test.rb 가 생성됨.
   ```

2. 테스트 스크립트 작성 위에 예시로 적은 로그인 처리를 확인하는 코드 예시는 아래와 같다.

   ```text
   require 'test_helper'
   class AdminLoginTest < ActionDispatch::IntegrationTest
     test "login test" do
       get '/hello/view'
       assert_response :redirect
       assert_redirected_to controller: :login, action: :index
       assert_equal '/hello/view', flash[:referer]
    
       follow_redirect!
       assert_response :success
       assert_equal '/hello/view', flash[:referer]
    
       post '/login/auth', { username: 'arint', password: '12345', referer: '/hello/view' }
       assert_response :redirect
       assert_redirected_to controller: :hello, action: :view
       assert_equal users(:arint).id, session[:usr]
    
       follow_redirect!
       assert_response :success
     end
   end 
   ```

   중간 중간에 있는 `follow_redirect!` 는 응답에 있는 리다이렉트 대상으로 다시 요청하는 기능이다. 미인증 상태의 사용자가 `/hello/view`에 접근하면 로그인 페이지로 리다이렉트하라는 응답을 받게 될텐데, 이 때 받은 리다이렉트 대상으로 이동하는 것을 모방하는 것이다.

3. 테스트 스크립트 실행

   `rake test:integration` 명령어를 사용하면 테스트를 실행시킬 수 있다.

