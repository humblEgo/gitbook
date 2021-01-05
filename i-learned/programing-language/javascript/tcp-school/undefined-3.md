# 연산자

동등연산자와 일치 연산자를 체크하자.

| 비교 연산자 | 설명 |
| :--- | :--- |
| `===` | 왼쪽 피연산자와 오른쪽 피연산자의 값이 같고, 같은 타입이면 참을 반환. |
| `==` | 왼쪽 피연산자와 오른쪽 피연산자의 값이 같으면 참을 반환. |
| `!=` | 왼쪽 피연산자와 오른쪽 피연산자의 값이 같지 않으면 참을 반환. |
| `!==` | 왼쪽 피연산자와 오른쪽 피연산자의 값이 같지 않거나, 타입이 다르면 참을 반환. |

자바스크립트에서 피연산자가 둘 다 문자열이면, 문자열의 첫 번째 문자부터 알파벳 순서대로 비교한다.

비트 연산자, 삼항 연산자, delete, typeof 연산자도 C, C++이랑 동일하다.

instanced 연산자는 피연산자인 객체가 특정 객체의 인스턴스인지 아닌지를 확인해준다.

```text
var str = new String("이것은 문자열입니다.");
​
str instanceof Object;  // true
str instanceof String;  // true
str instanceof Array;   // false
str instanceof Number;  // false
str instanceof Boolean; // false
```

void 연산자는 피연산자로 어떤 타입이 오던지 상관없이 언제나 undefined 값만을 반환한다.

