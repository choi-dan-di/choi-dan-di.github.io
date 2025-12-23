---
title: "[ASM] 변수 설정 & 메모리와 레지스터"
excerpt: "변수를 설정해보고 메모리의 개념, 메모리와 레지스터 간의 데이터 이동, 섹션(data, bss)에 대해 알아보기"

categories:
  - Assembly Language
tags:
  - [study, asm, assembly, section, memory, register, data, bss]

permalink: /asm/reg-to-memory/

toc: true
toc_sticky: true

date: 2022-11-02 00:00:00+0900
last_modified_at: 2022-11-02 00:00:00+0900
---

## 👻 메모리란?
**주기억장치** 또는 **컴퓨터 메모리(computer memory)**는 말 그대로 컴퓨터에서 수치·명령·자료 등을 _기억_ 하는 컴퓨터 하드웨어 장치를 가리킨다.   
보통 우리가 알고 있는 **RAM(Random Access Memory)**으로도 불리며 시스템에서 많은 프로그램을 실행할수록 더 많은 메모리가 필요하게 된다.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/memory-structure.png)   

위 그림은 메모리 구조를 나타내고 있다. 저번 포스팅에 컴퓨터 구조와 실행 파일 구조에 대해 이야기 하면서 한 번 얘기했었지만, 실행 파일을 실행시켰을 때 파일의 정보들이 메모리에 올라가게 되는데, 실행 파일 데이터 뿐만 아니라 다른 여러가지 데이터도 메모리에 같이 올라가게 된다. 우리가 직접 입력한 프로그램 코드, 힙, 스택 등의 데이터도 메모리에 함께 올라가게 된다.   

이번 시간엔 변수를 저장하는 **section .data**와 **section .bss**에 대해 알아보도록 하자.

***

## 👻 변수 저장소
어셈블리어를 사용할 때는 변수를 아무곳에서나 선언할 수 없다. 그대신 우리가 변수를 설정하고 저장할 수 있도록 **section .data**와 **section .bss** 영역이 할당되어있다. 변수를 저장하고 싶으면 이 영역에서 선언하면 된다.   

변수는 **초기화 된 데이터**와 **초기화 되지 않은 데이터** _두 종류_ 로 나뉘어지며 각 종류에 따라 저장 섹션도 달라진다.   
section .data에는 초기화 된 데이터가 할당되고, section .bss에는 초기화 되지 않은 데이터가 할당된다.   
> <u>초기화 된 데이터</u>라 함은 변수를 선언할 때 초기값을 입력해서 처음부터 값이 있는 상태로 초기화 해준다는 뜻이고, <u>초기화 되지 않은 데이터</u>는 말 그대로 초기값을 설정하지 않은 변수를 의미한다. 초기화 되지 않은 변수는 기본으로 **0**의 값을 가진다.

***

### 🌱 section .data
해당 영역에는 **초기화 된 데이터**가 들어간다. 변수 선언은 다음과 같다.   

```
[변수이름] [크기] [초기값]
크기는 db(1), dw(2), dd(4), dq(8) 이렇게 4개가 있다. (단위는 Byte)
```

변수를 선언하는 방법을 알았으니 이제 직접 입력해보자.

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/section-data.PNG)   

입력하는 데는 크게 어렵지 않다. 변수의 크기만 신경 써주면 무리없이 진행할 수 있다.   

***

### 🌱 section .bss
해당 영역에는 **초기화 되지 않은 데이터**가 들어간다. 변수 선언은 다음과 같다.

```
[변수이름] [크기] [개수]
크기는 resb(1), resw(2), resd(4), resq(8) 이렇게 4개가 있다. (단위는 Byte)
```

이 부분도 직접 변수를 선언하고 어떻게 할당이 되는 지 알아보자.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/section-bss.PNG)   

data 영역에 할당되는 변수와 다르게 마지막 부분에는 변수의 _개수_ 를 입력받게 된다. 그 차이 빼고는 딱히 다를 게 없다.

***

### 🌱 왜 data와 bss로 영역이 나뉘어졌을까?
사실 두 데이터 간의 차이는 초기화 유무밖에 없다. 하지만 실행 파일을 생각해 봤을 때, 실행 파일의 data 영역의 데이터는 어디에서도 알아야 하기 때문에 파일 내에도 한 공간을 차지하지만 bss 영역의 데이터는 초기화 되지 않고 자동으로 기본값 0이 세팅되며 **실행 파일 공간을 줄일 수 있다는 장점**이 있다. 요즘이야 컴퓨터 환경이 많이 개선되고 좋아졌지만 과거로 조금만 더 올라가보면 메모리 비용이 저렴하지는 않았다. 그래서 최대한 파일의 크기를 줄이기 위해 두 영역을 나누게 되었다. 참고는 해두자!

***

## 👻 Debug
이제 값이 제대로 들어갔는 지 확인을 해보기 위해 위에서 작성한 코드를 디버깅해보자.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/debug-1.PNG)   

여기서는 메모리 창을 열어주어야 한다. ``` Debug 👉 Show memory ```를 클릭하거나 단축키 **Ctrl+M**으로 열어주고, 우리가 설정했던 변수에 값이 제대로 들어갔는 지 확인하기 위해 **Add variable...**부분을 더블클릭하여 변수 이름을 입력해주자. 타입은 각각 표현을 어떻게 해 줄 것이냐를 의미하는데 왼쪽부터 차례로 **데이터 표기 방식**, **데이터 단위**, **보여줄 데이터 길이**를 의미한다. **Address**는 메모리의 주소를 나타내므로 지금은 체크해제 해주자.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/memory-result.PNG)   

> 여기선 16진수(Hex), 1 Byte단위, 12개의 데이터를 보여달라는 의미이다.   

우리가 입력해준 데이터들이 각 변수에 잘 담겼다는 것을 알 수 있다. 다시 본론으로 돌아가 메모리와 레지스터 간의 데이터 이동방식을 확인해보자.

***

### 🌱 메모리와 레지스터
우선 본문에 ```mov rax, a``` 를 추가해주자.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/debug-2.PNG)   

그리고 해당 코드에 브레이크 포인트를 걸고 디버그를 실행시켜서 커서를 위치시켜주고, F10을 눌러 한 줄 디버그를 실행시켜보자. 예상대로라면 변수 a에 담겨져 있는 ```0x11```이라는 값이 레지스터 rax 안에 들어가게 될 것이다.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/debug-3-reg-result.PNG)   

예상과는 다르게 ```0x403010```이라는 값이 저장되었다. 왜 그럴까?   
우선 변수에 초기값을 지정하게 되면 메모리에 값이 올라간다는 것을 알 수 있다. 메모리에 올라간 값을 레지스터로 이동시키라는 명령어를 만나게 되면 해당 값이 레지스터에 저장되는 게 아니라 **해당 값이 저장된 메모리의 주소값**이 올라가게된다. 고로 a의 메모리 주소값이 rax에 저장되는 것.   
rax에 저장된 값을 복사해 메모리 구간에 입력해서 확인해보자. <u>주소값</u>이라고 했으니 이번엔 우측 **Address에 체크**해야한다.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/memory-3.PNG)   

a의 값이 그대로 출력되는 것을 확인할 수 있다.   

> 메모리는 각각의 고유한 주소값을 가진다. 우리가 위에서 변수 a의 메모리 주소값을 찾을 수 있었는데, a의 값을 살펴보면 a값 뒤에 b값, c값이 차례대로 저장되어 있다는 것을 알 수 있다. 고로 a의 주소값에 1씩 더해주면 각각 b의 주소값, c의 주소값을 구할 수 있게 된다.   

그렇다면 레지스터에 메모리 주소값 대신 데이터 자체를 넣으려면 어떻게 해야할까?   
바로 변수를 ```대괄호([])```로 감싸주면 된다.

```
mov rax, [a]    ; a의 데이터를 rax에 저장해달라는 의미
```

기존의 ```mov rax, a``` 코드를 ```mov rax, [a]```로 변경해주고 하단 ```xor rax, rax```에 브레이크 포인트를 잡은다음 디버그해보자.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/debug-4.PNG)   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/debug-4-reg-result.PNG)   

rax에 ```0x4433333333222211```라는 값이 들어가있다. 뭔가 11이 나와서 맞게 나온 것 같은데 앞의 숫자들은 어디서 나왔는지 모르겠다. 메모리의 값을 가져올 때 해당 메모리에 있는 값만 가져오는 게 아닌 주변의 메모리까지 다같이 건드려지게 되는 것 같다. 정확한 값을 가져오기 위해 저장할 레지스터의 크기와 복사할 데이터의 크기를 맞춰주고 rax값을 0으로 초기화시키고 다시 해보자.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/debug-5.PNG)   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/debug-5-reg-result.PNG)   

값이 잘 나온다. ☺

***

반대로도 값을 복사할 수 있다. 우선 상수를 a 변수로 복사해보자.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/cst-to-variable.PNG)   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/error-msg.PNG)   

이렇게 그냥 쓰면 크기 에러가 난다. ```0x55```가 그냥 ```0x55```일지 ```0x000000000055```일지 모르기 때문에 에러가 나게된다. 그러니 크기를 꼭 지정해주도록 하자.   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/cst-to-variable-2.PNG)   

![Alt Text](/assets/images/posts_img/basics/asm/reg-to-memory/cst-to-variable-2-result.PNG)   

값이 잘 들어간 것을 확인할 수 있다. ☺☺☺   

물론 ```레지스터 👉 변수```도 가능하다.   

```
mov [a], cl     ; 크기가 같아 레지스터에는 굳이 크기 지정을 해주지 않아도 된다.
```

> 💡   
변수의 크기보다 더 큰 크기의 값을 넣으면 a 메모리 다음에 있는 b 메모리까지 침범하여 데이터를 저장하게 되니 참고하자.


***

## 👻 글을 마치며
이번 시간엔 메모리와 레지스터 간의 데이터 이동을 학습해보았다. 눈으로 보이지 않아 막연하게만 한다고 알고 있었는데, 눈으로 직접 보면서 실습을 진행하니 개념 정리가 잘 된 것 같다. 아직 크기 단위가 헷갈리지만 앞으로 계속 복습하다보면 금세 외울 수 있을 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_assembly/blob/master/register/reg-to-memory.asm)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   