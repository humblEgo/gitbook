# 컨트롤러 개발 - 요청 정보 추출

## 요청 정보 추출 - params 메서드

클라이언트에서 전달된 요청 정보에 params\[:&lt;매개 변수 이름&gt;\]의 형식으로 접근할 수 있다. params 메서드에서 추출할 수 있는 요청 정보는 아래와 같다.

| 종류 | 설명 |
| :--- | :--- |
| 포스트 데이터 | &lt;form method="POST"&gt;로 정의된 입력 양식에서 전달된 값 |
| 쿼리 정보 | URL 끝에 "?"이 붙고, "&lt;키 값&gt;=&lt;값&gt;&.."형식으로 지정된 정보 |
| 라우트 매개 변수 | 라우트에서 정의한 매개 변수\("/books/1"에서 "1" 부분\) |

params 메서드로 배열을 전달할 때는 다음과 같이 키 이름의 뒤에 를 붙여줘야한다.

```text
~/ctrl/para_array?category[]=rails&category[]=ruby
```

## 대량 할당 취약성을 피하는 방법

대량 할당\(Mass Assignment\)는 액티브 레코드에 있는 기본적인 기능 중 하나로, 모델의 필드를 한꺼번에 설정하는 것을 의미한다.

만약 아래처럼 new, update\_attributes 등의 메서드에 "&lt;필드 이름&gt;: &lt;값&gt;" 형태로 구성된 해시를 전달하면 해당 속성을 사용해 객체를 설정하는 방법을 쓴다고 가정해보자.

```text
@book = Book.new(params[:book])
@book = Book.find(params[:id])
@book.update_attributes(params[:book])
```

이 경우 우리 의도와 달리 params에 role 등의 권한과 관련된 필드를 수정하는 해시가 포함되면 보안문제가 발생할 소지가 크다. 이를 **대량 할당 취약성**이라고 부른다.

Rails는 이를 막기 위해 StrongParameters를 제공한다.

#### StrongParameters

StrongParameters는 대량 할당 취약성에 대한 화이트 리스트 대책 방법이다. 필드 값을 일괄 설정하기 전에 설정해도 괜찮은 값을 명시적으로 입력해 주는 것이 필요하다.

스캐폴딩으로 앱을 만들었을 때 컨트롤러에 포함되는 `params.require(model).permit(attr, ..)` 이 StrongParameters이다.

```text
def book_params
  params.require(:book).permit(:isbn, :title, :price, :publish, "cd")
end
```

## 요청의 다양한 정보 추출

한편 headers 메서드를 사용하면 요청에 포함된 헤더를 쉽게 추출할 수 있다.

```text
def req_head
  render text: request.headers['User-Agent']
end
```

업로드된 파일을 추추할 떄도 params 메서드를 사용할 수 있다. 이 경우 params 메서드는 업로드된 파일을 객체로 리턴한다.

1\) 파일 시스템에 파일을 업로드하는 경우 예시

폼에 multipart 옵션을 지정해서 파일을 업로드하자. 그 뒤 아래처럼 받으면 된다.

```text
def upload_process
  file = params[:upfile]
  name = file.original_filename
  perms = ['.jpg', 'jpeg', '.gif', '.png'] # 사용가능한 확장자 정의
  if !perms.include?(File.extname(name).downcase)
    result = '이미지 파일만 업로드해주세요!'
  elif file.size > 1.megabyte
    result = '1MB 이하의 파일만 업로드해주세요!'
  else
    File.open("public/docs/#{name}", 'wb') { |f| f.write(file.read) }
    result = "#{name.toutf8}를 업로드했습니다."
  end
    render text: result
end
```

2\) 데이터베이스에 파일을 업로드하는 경우 예시

데이터베이스에 크기가 큰 바이너리 데이터를 저장하는 것에 대해서는 장단점을 따져야한다.

* 장점: 파일 접근 제약을 걸 때 데이터베이스의 기능을 그대로 활용할 수 있다.\(파일 시스템으로 걸려면 번거로움.\)
* 단점: 데이터베이스 자체의 크기가 비대화된다.

