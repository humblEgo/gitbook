# 반복문

### for / in 문

for / in 문은 해당 객체의 모든 열거할 수 있는 프로퍼티\(enumerable properties\)를 순회할 수 있도록 해준다. 다시 한번 말하지만, for / in 문은 해당 객체가 가진 모든 프로퍼티를 반환하는 것이 아닌, 오직 열거할 수 있는 프로퍼티만을 반환한다.

```text
열거할 수 있는 프로퍼티란 내부적으로 enumerable 플래그가 true로 설정된 프로퍼티를 의미합니다. 이러한 프로퍼티들은 for / in 문으로 접근할 수 있게 됩니다.
```

### for / of 문

for / of 문은 반복할 수 있는 객체\(iterable objects\)를 순회할 수 있도록 해주는 반복문이다.

자바스크립트에서 반복할 수 있는 객체에는 Array, Map, Set, arguments 객체 등이 있다.

for / of 문은 익스플로러에서 지원하지 않는다.

### 루프 제어 - label 문

일반적으로 표현식의 검사를 통해 루프로 진입하면, 다음 표현식을 검사하기 전까지 루프 안에 있는 모든 실행문을 실행한다. 하지만 continue 문과 break 문은 이러한 일반적인 루프의 흐름을 사용자가 직접 제어할 수 있게 해준다.

label 문을 사용하면 continue 문과 break 문의 동작이 프로그램의 흐름을 특정 영역으로 이동시킬 수 있다. 문법은 아래와 같다.

```text
label:
식별하고자 하는 특정 영역
```

예시는 아래와 같다.

예시1\) 홀수에 대해서만 구구단 적용.

```text
gugudan:
for (var i = 2; i <= 9; i++) {
    dan:
    for (var j = 1; j <= 9; j++) {
        if ((i*j) % 2 == 0)
            continue dan;
        document.write(i + " * " + j + " = " + (i*j) + "<br>");
    }
}
```

예시2\) 3단까지만 구구단 적용

```text
gugudan:
for (var i = 2; i <= 9; i++) {
    dan:
    for (var j = 1; j <= 9; j++) {
        if (i > 3)
            break gugudan;
        document.write(i + " * " + j + " = " + (i*j) + "<br>");
    }
}
```

