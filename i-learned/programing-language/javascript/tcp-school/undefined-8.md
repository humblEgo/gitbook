# 함수 - 스코프와 호이스팅

## 변수 스코프

### **지역 변수\(local variable\)**

함수 내에서 선언된 변수. 변수가 선언된 함수 내에서만 유효하며, 함수가 종료되면 메모리에서 사라진다.

### **전역 변수\(global variable\)**

전역 변수\(global variable\)란 함수의 외부에서 선언된 변수를 가리킨다. 이러한 전역 변수는 프로그램의 어느 영역에서나 접근할 수 있으며, **웹 페이지가 닫혀야만 메모리에서 사라진다.**

자바스크립트에서 지역 변수를 선언할 때에는 반드시 var 키워드를 사용하여 선언해야 한다. **함수 내부에서 var 키워드를 사용하지 않고 변수를 선언하면, 해당 변수는 지역 변수가 아닌 전역 변수로 선언된다.**

또한, 전역 변수와 같은 이름의 지역 변수를 선언하면, 해당 블록에서는 해당 이름으로 지역 변수만을 호출할 수 있다.

물론 이 경우엔 전역 변수가 window 객체의 프로퍼티임을 명시하면 전역 변수를 호출할 수도 있다.

## 함수 스코프

블록\(block\)이란 코드 내에서 중괄호\({}\)로 둘러싸인 부분을 가리키며, 대부분의 프로그래밍 언어에서는 블록 내에서 정의된 변수를 블록 외부에서는 접근할 수 없다.

하지만 자바스크립트는 다른 언어와 달리 함수를 블록 대신 사용한다.

'전역 함수'는 모든 전역 변수와 전역 함수에 접근할 수 있다.

반면, 다른 함수 내에 정의된 '내부 함수'는 그 함수의 부모 함수\(parent function\)에서 정의된 모든 변수 및 부모 함수가 접근할 수 있는 모든 다른 변수까지도 접근할 수 있다.

아래 예시를 참고.

```text
// x, y, name을 전역 변수로 선언함.
var x = 10, y = 20;
// sub()를 전역 함수로 선언함.
function sub() {
    return x - y;     // 전역 변수인 x, y에 접근함.
}
document.write(sub() + "<br>");
// parentFunc()을 전역 함수로 선언함.
function parentFunc() {
    var x = 1, y = 2; // 전역 변수와 같은 이름으로 선언하여 전역 변수의 범위를 제한함.
    function add() {  // add() 함수는 내부 함수로 선언됨.
        return x + y; // 전역 변수가 아닌 지역 변수 x, y에 접근함.
    }
    return add();
}
document.write(parentFunc());
```

## 함수 호이스팅\(hoisting\)

자바스크립트에서 함수의 유효 범위라는 것은 함수 안에서 선언된 모든 변수는 함수 전체에 걸쳐 유효하다는 의미이다.

그런데 이 유효 범위의 적용이 변수가 선언되기 전에도 똑같이 적용된다. 이러한 자바스크립트의 특징을 함수 호이스팅\(hoisting\)이라고 한다. 즉, **자바스크립트 함수 안에 있는 모든 변수의 선언은 함수의 맨 처음으로 이동된 것처럼 동작**한다.

아래 예제를 보자.

```text
var globalNum = 10;     // globalNum을 전역 변수로 선언함.
​
function printNum() {
    document.write("지역 변수 globalNum 선언 전의 globalNum의 값은 " + globalNum + "입니다.<br>"); // ①
    var globalNum = 20; // globalNum을 지역 변수로 선언함. // ②
    document.write("지역 변수 globalNum 선언 후의 globalNum의 값은 " + globalNum + "입니다.<br>");
}
​
printNum();
```

①에서 전역변수를 참조할 것 같지만 아니다. 호이스팅이 적용되어 아래처럼 코드가 바뀌기 때문.

```text
var globalNum = 10;     // globalNum을 전역 변수로 선언함.
​
function printNum() {
    var globalNum; // 함수 호이스팅에 의해 변수의 선언 부분이 함수의 맨 처음 부분으로 이동됨.
    document.write("지역 변수 globalNum 선언 전의 globalNum의 값은 " + globalNum + "입니다.<br>"); // ①
    globalNum = 20; // globalNum을 지역 변수로 선언함. // ②
    document.write("지역 변수 globalNum 선언 후의 globalNum의 값은 " + globalNum + "입니다.<br>");
}
​
printNum();
```

