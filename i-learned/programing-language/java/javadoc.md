# JavaDoc에 대해

## JavaDoc의 `description`과 `@return`에 내용 중복을 제거해야할까?

JavaDoc을 작성하다보면 `description` 에 적은 내용을 `@return`에도 적는 경우가 생긴다. 중복을 싫어하는 개발자라면 동일한 내용을 중복해서 작성하는 것이 바람직한 것인지 의문이 생길 수 있다.

### **그 이유는**

1. `description`과 `@return`의 역할이 애초에 다르기 때문이다.

   * `description`: 메소드의 기능과 역할을 설명
   * `@return`: 리턴 값을 설명

   각 역할에 충실하게 내용이 적혀있어야 원하는 정보를 빠르게 확인할 때 도움 받을 수 있다.

2. JavaDoc으로 웹 페이지를 빌드하면 화면 최상단에는 `@return`은 표시되지 않는 반면 `description`이 표시된다. 따라서 `description`에 리턴 값에 대한 설명이 포함된다면 굳이 메서드 상세 설명을 확인하지 않아도 빠르게 메서드에 대해 파악할 수 있다.

[구글 자바 스타일 가이드](https://google.github.io/styleguide/javaguide.html#s7.2-summary-fragment)에서도 비슷한 맥락의 설명이 있다.확인해보자.

### 조금더 구체적인예시

아래 예시는 반환 값에 대한 설명이 `description`과 `@return`에 중복되지 않도록 적은 것이다. \(비추천\)

```text
    /**
     * 상품 목록을 조회한다.
     *
     * @return 상품 목록.
     */
```

아래 예시는 반환 값에 대한 설명이 `description`과 `@return`에 중복되지 않도록 적은 것이다. \(추천\)

```text
    /**
     * 상품 목록을 조회해 리턴한다.
     *
     * @return 상품 목록
     */
```

실제로 [String 클래스의 JavaDoc](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html)을 보면, `description`에도 반환값에 대한 설명이 포함되어 있다. 그 중 `charAt` 메서드를 보자.

```text
Returns the char value at the specified index. An index ranges from 0 to length() - 1. The first char value of the sequence is at index 0, the next at index 1, and so on, as for array indexing.
If the char value specified by the index is a surrogate, the surrogate value is returned.
​
Specified by:
charAt in interface CharSequence
Parameters:
index - the index of the char value.
Returns:
the char value at the specified index of this string. The first char value is at index 0.
Throws:
IndexOutOfBoundsException - if the index argument is negative or not less than the length of this string.
```

### 물론 이건 원칙은 아니다.

> 단순한 메소드는 두 문장이 중복이 되기도 해요. 저는 이렇게 중복이 되는 경우는 그냥 description만 남겨두기도 합니다. 지금 강조하는 것은 JavaDoc을 작성하는 훈련을 위한 것이고요. - johngrib님

### 참고

* [markruler님의 질문과 johngrib님의 답변](https://github.com/CodeSoom/spring-week5-assignment-1/pull/17)

