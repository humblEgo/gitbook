---
description: Node.js를 학습한 내용 메모
---

# Node.js & Express

### **쓰레드 기반 vs 비동기 이벤트 기반**

대부분의 애플리케이션은 Blocking I/O를 사용하여 멀티 쓰레드를 사용할 수 밖에 없었다.

* Blocking I/O는 **하나의 프로세스가 어떤 자원을 사용하고자 할 때 그 자원을 다른 프로세스가 점유하고 있다면, 그 프로세스가 그 자원의 사용을 끝마칠 때까지 기다려야 한다는 것**을 의미한다.
* 문제는 **Blocking I/O 자체가 쓰레드 지연을 발생시킨다는 것이다.** I/O 요청을 하고 응답이 올 때까지 아무것도 하지 않고 시간을 낭비하게 된다!
* 또한 **스케쥴링 자체에 걸리는 시간과 쓰레드가 많아질 수록 늘어나는 Context switch 비용도 무시할 수 없다.**
* 대안으로는 쓰레드들을 별도로 관리하거나, 더 작은 단위로 쪼개어 VM 등으로 실제 쓰레드로 분배하는 방식 같은 대안이 등장한다. 그리고 노드의 방식도 대안이다. 어떤 식이냐면..

### ..싱글 쓰레드와 이벤트 기반의 비동기 I/O 처리

노드는 이러한 문제들을 **싱글 쓰레드**와 **이벤트 기반의 비동기 I/O 처리로 해결**하고 있다. 노드는 I/O 작업이 시작되면 I/O 작업 처리에 대한 응답을 기다리지 않고, **바로 다음 작업을 실행**해버린다. 대신 I/O 작업이 종료되면 이벤트를 발생시키고, 이 이벤트는 해당 프로세스의 이벤트 큐에 등록되게 된다. 노드로 개발된 프로세스는 이 이벤트 큐에 등록된 새로운 이벤트를 감지하여, 해당 이벤트 시 수행하여야 할 작업을 실행하게 된다.

### 이벤트 루프

이벤트 루프\(Event Loop\)라는 것은 작업을 요청하면서 그 작업이 완료되었을 때 어떤 작업을 진행할지에 대한 콜백 함수를 지정하여 **동작이 완료되었을 때 해당 콜백 함수를 실행되는 방식의 동작 방식**을 말한다.

이런 방식이기 때문에 **이벤트 루프는 어떤 요청이 발생하면 그 작업에 대해 쓰레드 실행만을 일으킬 뿐**이다.

![image](https://user-images.githubusercontent.com/54612343/113382132-b765c680-93bb-11eb-85b7-566952004d7b.png)

jekim님과 관련해서 토론하다가 이 [링크](https://velog.io/@gtah2yk/-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%8B%9C%EA%B0%81%ED%99%94-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84-7yk5qv46ur)를 공유 받았다.시각적으로 이벤트 루프 작동방식을 확인할 수 있다!

### Node.js의 아키텍쳐

![image](https://user-images.githubusercontent.com/54612343/113382224-f85ddb00-93bb-11eb-9090-2ac0a0940625.png)

초창기의 Node.js 아키텍쳐.

![image](https://user-images.githubusercontent.com/54612343/113382318-2cd19700-93bc-11eb-8b29-1cc159710cd5.png)

지금은 기술이 발전하여 위처럼 libev의 종속성을 제거하고 libeio와 libev를 쓰는 버전으로 변경되었다.

각 모듈별 역할은 이 [링크](https://edu.goorm.io/learn/lecture/557/%ED%95%9C-%EB%88%88%EC%97%90-%EB%81%9D%EB%82%B4%EB%8A%94-node-js/lesson/174356/node-js%EC%9D%98-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90)를 참고.



### Node Beginner Book

node.js에서의 비동기처리를 학습할 수 있는 좋은 링크!  
다른 분들에게도 자신있게 추천할 수 있는 링크이다.

* [nodebeginner.org](https://www.nodebeginner.org/index-kr.html#how-to-not-do-it)
* [okdevtv.com/mib/nodejs](https://okdevtv.com/mib/nodejs)

