# invocation에 대해서

코드숨 4주차 강의를 복습하는데, 아래 예제에서 나온 invocation 단어가 낯설게 느껴졌다.

```text
given(taskRepository.save(only(Task.class)).will(invocation -> {
  return invocation.getArgument(0);
}))
```

이 단어는 다른 [BDDMockito 예제](https://www.baeldung.com/bdd-mockito#2-returning-a-dynamic-value)에서도 자연스럽게 나온다.

```text
given(phoneBookRepository.contains(momContactName))
  .willReturn(true);
given(phoneBookRepository.getPhoneNumberByContactName(momContactName))
  .will((InvocationOnMock invocation) ->
    invocation.getArgument(0).equals(momContactName) 
      ? momPhoneNumber 
      : null);
phoneBookService.search(momContactName);
then(phoneBookRepository)
  .should()
  .getPhoneNumberByContactName(momContactName);
```

일단 네이버 사전에서는 영 알 수 없는 뜻이 나온다.

> invocation
>
> * **1.**\(신을 향한\) 기도\[기원\], \(권한을 지닌 이를 향한\) 탄원\[호소\], \(혼·영감 등을 비는\) 주문\(呪文\)\[기도\]
> * **2.**\(법적 권한 등의\) 발동\[실시\]

조금 더 찾아보니 [Java API 문서](https://docs.oracle.com/javaee/7/api/javax/ws/rs/client/Invocation.html)에 인터페이스로 명시되어 있었다.

> ```text
> public interface Invocation
> ```
>
> A client request invocation. An invocation is a request that has been prepared and is ready for execution. Invocations provide a generic \(command\) interface that enables a separation of concerns between the creator and the submitter. In particular, the submitter does not need to know how the invocation was prepared, but only how it should be executed \(synchronously or asynchronously\) and when.

그리고 [스택오버플로우](https://stackoverflow.com/questions/2486885/java-method-invocation-execution)에도 아래 내용이 들어있는 글을 찾을 수 있었다.

> * _invocation_ is the event of issuing the call to the method; technically - placing the method onto the stack
> * _execution_ is the whole process of running the method - from invocation till completion. And _execution time_ is period during which the method body runs.

정리하면

* invocation은 execution의 전단계로써 메서드를 호출하는 개념이다.
* 제네릭 인터페이스를 제공한다.
* creator와 submitter의 관심을 분리시킬 때 요긴하게 쓰인다. submitter는 invocation이 어떻게 준비되는지 알 필요가 없다. 그냥 invocation을 실행시킬 때 필요한 정보만 알고 있으면 되는 것!

