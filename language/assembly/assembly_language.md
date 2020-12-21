# Assembly\_language

어셈블리어\(Assembler language\)는 [기계어](https://github.com/humblEgo/TIL/blob/master/Language/Assembly/assembly_language.md#%EA%B8%B0%EA%B3%84%EC%96%B4)와 1:1 대응이 되는 컴퓨터 프로그래밍의 저급언어\(low-level programming language\)이다. 컴퓨터 구조에 따라 사용하는 기계어가 달라지므로, 1:1 대응이 되는 어셈블리어도 각각 다르게 된다.

어셈블리 언어는 오래된 언어이고 잘 쓰이지 않을 것 같지만 두 가지 관점에서 여전히 살아있는 언어이다. 첫째는 보안이라는 관점에서 볼 때 소스가 공개되지 않는 프로그램을 바이너리수준에서 분석하고 이해해야 한다면 디스어셈블 된 코드를 읽어야 하는데, 이때 어셈블리 언어에 대한 이해가 없다면 불가능하다. 둘째는 하드웨어를 직접 제어 하는 부분\(드라이버나 운영체제 커널의 특별한 모듈\)은 여전히 그 간결성과 편의성 때문에 어셈블리 언어로 코딩을 할 수밖에 없다. 예를 들어 운영체제 소스를 분석해 보면 여전히 소스 중 일정비율은 어셈블리 언어로 코딩되어 있다.

어셈블리 언어는 정해진 표준이 없다. AT&T 방식과 Intel 방식이 대표적으로 쓰인다. 둘 모두 opcode\(명령어\)와 operand\(인자값, 피연산자\)로 이루어져있다는 공통점이 있다. 차이점은 아래 표와 같으며 서로 호환되지 않는다.

| 항목 | AT&T | Intel |
| :--- | :--- | :--- |
| 접두사 규칙 | 레지스터는 `%`, 값은 `$` | 16진수 데이터는 `h`, 2진수 데이터는 `b` |
| operands 위치 차이 | destination operands가 뒤, source operands가 앞 | dest operands가 앞, src operands가 뒤 |
| memory operands | `(`와 `)` 사이에 놓음 | `[`와 `]` 사이에 놓음 |
| 접미사 규칙 | operands size 등 고유의미가진 접미사 존재 | 접미사 사용않고 의미있는 문장을 그대로 사용함. |

## 개념

### 어셈블러

어셈블러\(assembler\)는 [니모닉 기호\(mnemonics\)](assembly_language.md)를 opcode로 변환하고 메모리 위치와 기타 존재물에 따라 식별자를 다시 분석함으로써 목적 코드를 만들어낸다.

### 니모닉 기호\(mnemonic\)

mnemonic의 사전적 의미는 **어떤 것을 기억하는 데 쉽게 하도록 도움을 주는 것, 또는 쉽게 기억되는 성질**이다. 그 어원은 아래와 같다.

> mnemonic 의 어원은 그리스 신화의 기억의 여신 므네모시네\(Mnemosyne\) 에서 유래 되었습니다. 므네모시네의 자녀인 뮤즈\(Muses\)는 올림포스 산의 신들의 축제에서 아폴론을 도와 음악을 연주 하였는데, 악보가 없는 세계라 기억력에 의존하여 연주할 수 있었다고 합니다.

어원에서 알 수 있듯이 mnemonic 자체는 꼭 어셈블리어에 대해서가 아니라 기억하기 어려운 것을 기억하기 쉽도록 만드는 모든 개념을 뜻한 것이라고 할 수 있다. 어셈블리어를 설명할 때 쓰이는 mnemonic 기호는 기계어로된 일련의 숫자를 인간이 알아보기 쉽게 영어로 바꾸어준 것을 뜻한다.

### 패스의 수

어셈블러에는 두 가지 종류가 있다. 실행 프로그램을 만들기 위해 얼마나 많은 패스가 소스를 거치는지에 따라 다르다.

* 1패스\(one-pass\) 어셈블러: 소스 코드를 한번만 거친다.
* 다중 패스\(multi-pass\) 어셈블러: 처음 패스들에서 모든 기호와 관련 값들이 포함된 테이블 하나를 만들고 나중 패스들에서 테이블을 이용하여 코드를 만들어낸다.

### 기계어

기계어는 특별한 변환 과정 없이 컴퓨터가 직접 처리할 수 있는 유일한 언어이다. 아래는 n번째 피보나치 수를 계산하는 32비트 x86 기계어를 표현한 16진 기능의 예이다.

```c
8B542408 83FA0077 06B80000 0000C383
FA027706 B8010000 00C353BB 01000000
C9010000 008D0419 83FA0376 078BD98B
B84AEBF1 5BC3
```

### 레지스터\(Register\)

레지스터는 CPU 바로 안에 있는 고속 저장 장소이며 일반 메모리보다 훨씬 빠른 속도로 접근할 수 있다. 프로그램이 실행되면 실행 파일이 메모리에 상주하면서 동작에 필요한 일부 데이터들이 CPU로 전달되게 되는데, 이 때 그 데이터들이 저장되는 곳이 레지스터이다.

기본적인 프로그램 실행 레지스터는 8개의 범용 레지스터와 6개의 세그먼트 레지스터, 프로세서 상태 플래그 레지스터와 명령어 포인터가 있다. 아래는 linux, FreeBSD, macOS 등에 적용되는 [System V AMD64 ABI의 호출규칙](https://en.wikipedia.org/wiki/X86_calling_conventions#x86-64_Calling_Conventions)에 따른 분류이다.

* RSP: 스택 포인터 레지스터. 스택에 push할 경우 -4h, pop할 경우 +4h로 값이 바뀐다\(스택은 큰 수의 메모리주소에서 작은 수로 자라나므로 push할 때 빼지고 pop할 때 더해진다고 생각하면 쉽다.\)
* RBP: 베이스 포인터 레지스터. RSP에 보조적으로 사용되는 포인터 레지스터이다. 스택을 가리키는데 RSP 하나로도 충분하지만, 프로그램의 오류로 중요한 스택 공간이 망가지는 일을 어느정도 방지하기 위해 쓰인다.
* RSI: source index 레지스터
* RDI: destination index 레지스터

64비트 이전에는 그냥 parameter를 전달했지만, 64비트 체제에서는 레지스터리를 이용해서 parameter를 전달한다. 64비트 체제에서 parameter 전달 방식은 운영체제에 따라 나누어진다. 그 중 리눅스의 경우 아래와 같다.

* RDI: 첫번째 인자
* RSI: 두번째 인자
* RDX: 세번째 인자
* RCX: 네번째 인자
* R8: 다섯번째 인자
* R9: 여섯번째 인자
* RAX: 산술\(덧셈, 곱셈, 나눗셈 등\), 논리 연산을 수행하며 **함수의 반환값**이 이 레지스터에 저장된다.
* RBX: ESI 레지스터나 EDI 레지스터와 결합될 수 있으며, 메모리 주소를 저장하기 위한 용도로 사용된다.
* RCX: 카운터 레지스터. 주로 반복 명령어 사용시 반복 카운터로 사용된다.
* RDX: EAX와 같이 쓰이고 부호 확장 명령 등에 쓰인다. 큰 수의 곱셈 또는 나눗셈 등의 연산이 이루어질 때, EDX 레지스터가 사용되어 EAX 레지스터와 함께 쓰인다.

### syscall

rax에 저장한 값으로 원하는 system 함수를 불러올 수 있다. [syscall table](https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/)을 참고할 것.

### 자주쓰는 명령어

* MOV: 좌변에 우변\(혹은 상수\)의 값을 입력한다. 목적지의 피연산자의 길이는 원시 피연산자의 길이와 반드시 일치해야한다.
* LEA: 좌변\(레지스터만 가능\)에 우변의 주소값을 입력한다.
* JMP: 원하는 주소로 이동하여 실행한다.
* JA, JB, JE 등등 : cmp, test 명령문으로 인해 값이 바뀌는 flag들에 따라 피연산자로 입력된 주소로의 이동여부를 결정한다.
* CALL: 함수를 호출했다가 다시 원래 위치로 돌아올 때 사용. JMP와 다른 점은 실행한 뒹 끝나게 되면 REI에 저장하고 다시 우너래 상태로 돌아온다는 점.
* NOP: 아무 작업도 하지 않는 명령어. 1byte의 빈 공간을 차지한다.
* RET: 현재 함수가 끝난 뒤에 돌아갈 주소를 지정하기 위해 사용.
* PUSH:스택에 해당 값을 넣음.
* POP: 스택에서 값을 뺴냄.
* LEAVE: 현재까지의 메모리 스택을 비우고 EBP를 자신을 호출한 메모리 주소로 채운다. 실행 중인 함수를 종료할 때 많이 쓴다.
* INC: 피연산자에 1을 더하는 명령어. _참고로_ [_Intel: Introduction to x64 Assembly_](https://software.intel.com/content/www/us/en/develop/articles/introduction-to-x64-assembly.html)_에 갑자기 등장하는 INX는 INC 명령어의 오타임이 분명하다.. 뭔지 찾느라 시간버리지말자._
* DEC: 피연산자에 1을 빼는 명령어.
* MOVZX: 부호없는 산술 값에 대해서 사용됨. 바이트나 워드를 워드나 더블워드 목적지에 전송하고, 나머지 목적지 피연산자의 비트에는 0으로 채움.
* MOVSX: 부호있는 산술 값에 대해서 사용됨. 바이트나 워드를 워드나 더블워드 목적지에 전송하고, 나머지 목적지 피연산자의 비트에는 '부호비트\(원시 피연산자의 맨 왼쪽비트\)'로 채움.

### pwndbg에서 cmp 연산의 결과값 확인하기

cmp는 두 피연산자를 비교하는 작업을 한다. dest 에서 src를 묵시적으로 빼서 값을 비교하는데, 값이 같다면 결과는 0이 되고 ZF\(Zero flag\)가 1이 된다. 다르다면 ZF가 0으로 세트된다. 처음 상태에는 NZ\(Not Zero\)로 ZF가 0으로 세트되어있는 상태이다.

pwndbg를 실행하고 있는 상태에서 `info register` 커맨드를 치면 해당 시점의 모든 register 값을 확인할 수 있다. 이 때 eflags를 확인하면 ZF flag를 확인할 수 있다. 이 커맨드는 `i r` 로 축약 가능하다.

* 참고
  * \[어셈블리어-위키피디아\]\([https://ko.wikipedia.org/wiki/%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC%EC%96%B4\#:~:text=%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC%EC%96%B4\(%EC%98%81%EC%96%B4%3A%20assembly%20language\),%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%98%20%EC%A0%80%EA%B8%89%20%EC%96%B8%EC%96%B4%EC%9D%B4%EB%8B%A4.](https://ko.wikipedia.org/wiki/%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC%EC%96%B4#:~:text=%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC%EC%96%B4%28%EC%98%81%EC%96%B4%3A%20assembly%20language%29,%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%98%20%EC%A0%80%EA%B8%89%20%EC%96%B8%EC%96%B4%EC%9D%B4%EB%8B%A4.)\)
  * [저급 프로그래밍 언어-위키피디아](https://ko.wikipedia.org/wiki/%EC%A0%80%EA%B8%89_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4)
  * [mnemonic이란 by henry kang](https://medium.com/hexlant/mnemonic-%EC%9D%B4%EB%9E%80-7fb48106bd77)\)
  * [Intel과 AT&T의 차이점](https://hardner.tistory.com/22)
  * [리버싱과 시스템 해킹을 공부하려면 꼭 알아야하는 어셈블리어의 기초-어셈블리어](https://www.youtube.com/watch?v=yf7yFJHTif8)
  * [태초의 프로그래밍 언어 어셈블리](https://github.com/gurugio/book_assembly_8086_ko/blob/master/README.md)
  * [레지스터의 종류와 특징](https://beomnaegol.tistory.com/entry/Assembly-%EB%A0%88%EC%A7%80%EC%8A%A4%ED%84%B0%EC%9D%98-%EC%A2%85%EB%A5%98%EC%99%80-%ED%8A%B9%EC%A7%95)
  * [어셈블리어 튜토리얼](https://note.heyo.me/asm-tutorial-0/)
  * [Introduction to x64 Assembly](https://software.intel.com/content/www/us/en/develop/articles/introduction-to-x64-assembly.html)
  * [NASM tutorial](https://cs.lmu.edu/~ray/notes/nasmtutorial/)
  * [Assembly on the Mas](https://stackoverrun.com/ko/q/4687127)
  * [범용 레지스터: EAX, ECX, EDX, ESI...](https://blog.hexabrain.net/186?category=468615)
  * [어셈명령어 mov와 lea 차이](https://debugjung.tistory.com/entry/%EC%96%B4%EC%85%88-%EB%AA%85%EB%A0%B9%EC%96%B4-mov%EC%99%80-lea-%EC%B0%A8%EC%9D%B4)
  * [어셈블리 명령어 정리-peemang IT Blog](https://peemangit.tistory.com/40?category=820239)
  * [How to print error message using errno in assembly language](https://stackoverflow.com/questions/28922572/how-to-print-error-message-using-errno-in-assembly-language/28952085)
  * [How to return errno in assembly?](https://stackoverflow.com/questions/15304829/how-to-return-errno-in-assembly)
  * [Conditional jump and flag bit in assembly](https://stackoverflow.com/questions/49772797/conditional-jump-and-flag-bit-in-assembly)
  * [MAC OS SYSTEM CALL TABLE](https://sigsegv.pl/osx-bsd-syscalls/)

