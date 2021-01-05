# 배열\(array\)

**자바스크립트에서 배열\(array\): 이름과 인덱스로 참조되는 정렬된 값의 집합**

배열을 구성하는 각각의 값을 배열 요소\(element\)라고 하며, 배열에서의 위치를 가리키는 숫자를 인덱스\(index\)라고 헌더,

 자바스크립트에서 배열의 특징은 다음과 같습니다.

1. 배열 요소의 타입이 고정되어 있지 않으므로, 같은 배열에 있는 배열 요소끼리의 타입이 서로 다를 수도 있다.
2. 배열 요소의 인덱스가 연속적이지 않아도 되며, 따라서 특정 배열 요소가 비어 있을 수도 있다.
3. 자바스크립트에서 배열은 Array 객체로 다뤄진다.

### 배열 생성

생성 방식은 3가지가 있으며 모두 같은 결과의 배열을 만들어 준다.

1. _var_ arr = \[배열요소1, 배열요소2,...\]; _// 배열 리터럴을 이용하는 방법_
2. _var_ arr = Array\(배열요소1, 배열요소2,...\); _// Array 객체의 생성자를 이용하는 방법_
3. _var_ arr = new Array\(배열요소1, 배열요소2,...\); _// new 연산자를 이용한 Array 객체 생성 방법_

### **배열 요소의 추가**

배열에 요소를 추가하는 방법은 3가지가 있다.

```text
1. arr.push(추가할 요소);         // push() 메소드를 이용하는 방법
2. arr[arr.length] = 추가할 요소; // length 프로퍼티를 이용하는 방법
3. arr[특정인덱스] = 추가할 요소; // 특정 인덱스를 지정하여 추가하는 방법
```

push\(\) 메소드와 length 프로퍼티를 이용한 방법은 모두 배열의 제일 끝에 새로운 요소를 추가한다.

인덱스에 대응하는 배열 요소가 없는 부분은 배열의 홀\(hole\)이라고 한다. 자바스크립트에서는 이러한 배열의 홀을 undefined 값을 가지는 요소처럼 취급한다.

### **희소 배열**

희소 배열이란 배열에 속한 요소의 위치가 연속적이지 않은 배열을 의미한다.

### **연관 배열**

숫자로 된 인덱스 대신에 문자열로 된 키\(key\)를 사용하는 배열을 연관 배열\(associative array\)이라고 한다. 자바스크립트는 연관 배열을 별도로 제공하지는 않지만, 인덱스로 문자열을 사용하여 연관 배열처럼 사용할 수 있는 객체\(object\)를 만들 수 있다.

이렇게 **생성된 배열은 자바스크립트 내부적으로 Array 객체가 아닌 기본 객체로 재선언된다.** 즉, 연관 배열은 Array 객체가 아니므로 length 프로퍼티의 값이 0 이고 숫자를 인덱스로 하여 요소에 접근하면 undefined를 확인하게 된다.

ECMAScript6 부터는 이러한 불편함을 해결하기 위해 새로운 데이터 구조인 Map 객체를 별도로 제공하고 있다.

### **자바스크립트에서 배열 여부 확인**

 자바스크립트에서는 해당 변수가 배열인지 여부를 확인할 수 있도록 다음과 같은 방법들을 제공하고 있다.

1. Array.isArray\(\) 메소드
2. instanceof 연산자
3. constructor 프로퍼티

또한 Array 객체의 constructor 프로퍼티를 사용하여 배열 여부를 확인할 수도 있다. 예시를 보면 된다.

```text
function isArray(a) {
​
    return a.constructor.toString().indexOf("Array") > -1;
​
}
​
var arr = [1, true, "JavaScript"];          // 배열 생성
document.write(arr.constructor);            // constructor 프로퍼티의 값 출력
document.write(arr.constructor.toString()); // function Array() {[native code]}
document.write(arr.constructor.toString().indexOf("Array")); // 10
document.write(isArray(arr))                // true
```

