---
title: "[ASM] 레지스터 기초"
excerpt: "mov 명령어, 레지스터 개념과 데이터 처리 방식에 대해 알아보기"

categories:
  - Assembly Language
tags:
  - [study, asm, assembly, mov, register]

permalink: /asm/reg-basic/

toc: true
toc_sticky: true

date: 2022-11-01 02:02:02+0900
last_modified_at: 2022-11-01 02:02:02+0900
---

## 👻 레지스터란?
프로세서에 위치한 <u>고속 메모리</u>로, 극히 **소량의 데이터**나 **처리 중인 중간 결과**와도 같은 _프로세서가 바로 사용할 수 있는 데이터를 담고 있는 영역_ 을 말한다.   
메모리 계층의 최상위에 위치하며, 가장 빠른 속도로 접근 가능한 메모리이다. 메인 메모리에 올려져있는 파일을 CPU가 처리할 때, 처리 과정에서 생겨난 일시적인 데이터들을 저장하는 곳이라고 보면 될 것 같다.   
참고로 레지스터는 하나의 저장소만 있는 게 아니라 여러개의 저장소가 있고 이를 통틀어 **레지스터(Register Set)**이라 한다.

> 왜 레지스터를 사용해야할까?   
👉 데이터를 저장할 수 있는 곳은 다양하다. (메인 메모리나 하드 디스크 등...)   
하지만 최종적으로 데이터를 처리하는 곳은 CPU의 다른 한 부분이고, 수없이 연산을 하면 방대한 중간 결과값들이 나올텐데 이왕이면 가까운 곳에 저장소를 하나 더 놔두고 그 때마다 잠깐씩 꺼내 쓰고 지우는 게 효율적이기 때문이다.☺   
레지스터에서 메모리로, 레지스터에서 CPU로 보내는 경우도 많아 비행기 조종실과 비슷하다고 생각하면 될 것 같다.

***

## 👻 레지스터 구조
![Alt text](/assets/images/posts_img/basics/asm/reg-basic/reg-structure2.png)   
기본적으로 범용 레지스터는 32비트 기준 8개가 존재한다. 위쪽에 있는 **EAX, EBX, ECX, EDX** 그리고 **EBP, ESP, ESI, EDI**가 있는데 왼쪽의 EAX, EBX, ECX, EDX는 주로 <u>산술 연산</u>을 할 때 사용되며 오른쪽 4개의 레지스터는 스택 메모리와 관련된 메모리 주소를 저장하는 <u>포인터</u>로 사용된다.   


![Alt text](/assets/images/posts_img/basics/asm/reg-basic/reg-structure.png)   
하나의 레지스터를 확대해보면 이렇게 나눌 수 있다.   
RAX는 64비트 환경에 존재하는 레지스터고, 64 bits의 크기를 가지고 있다. 그리고 그 반인 32 bits를 가지는 EAX, EAX의 반인 AX, AX의 반인 AH와 AL이 있다. 어떤 부위를 사용할 것인지에 따라 정해진 이름들이 있다.   
> 64비트 환경이면 레지스터의 크기도 64비트(8바이트)다.

***

## 👻 레지스터 실습해보기
이제 앞서 이론으로 습득했던 레지스터의 개념을 확실히 내 것으로 만들기 위해 SASM 툴의 ```mov```라는 명령어를 이용해 실습을 해보자.  

***

### 🌱 mov 사용해보기
```mov``` 명령어는 데이터를 이동시켜주는 함수다.   
입력값이 두 개가 필요하고 오른쪽에서 왼쪽으로 데이터를 이동시켜준다.   

```
mov reg1, cst
mov reg1, reg2
```

사용법은 위와 같고 각각 cst를 reg1에, reg2의 값을 reg1로 이동시켜달라는 의미이다.   

코드를 작성하고 한 번 확인해보자.   
![Alt text](/assets/images/posts_img/basics/asm/reg-basic/mov1.PNG)   
eax는 32비트 크기의 레지스터, rbx는 64비트, cl은 최하단 8비트짜리 레지스터다.   
크기가 정해져있는 레지스터를 작업하기 때문에 용량 확인을 위하여 cl에는 일부러 8비트를 넘어가는 수를 입력해보았다.   
![Alt text](/assets/images/posts_img/basics/asm/reg-basic/mov1-error.PNG)   
역시나 바이트 범위를 초과했다고 에러가 뜬다. ```0xff``` 값으로 수정하고 다시 실행시키면 에러없이 잘 된다.   

***

### 🌱 Debug
디버그는 코드가 버그가 있는지, 있다면 어느 부분에서 나타나는 건지를 알려주는 기능이다.   
해당 코드는 에러 없이 빌드가 잘 되었으니 디버그를 하게 된다면 각 값들이 어떻게 이동이 되는 지를 알 수 있다.   
상단의 **버그 표시가 있는 실행버튼**을 눌러서 디버그를 시작하거나 단축키 **F5**를 눌러서 시작하면 된다. (참고로 일반 실행은 단축키 **F9**이다.)   
![Alt text](/assets/images/posts_img/basics/asm/reg-basic/mov1-debug.PNG)   
그러면 페이지가 이렇게 뜨게 되는데, 이 상태에서 레지스터나 메모리 창을 열어 중간중간 값을 확인할 수 있다. 나는 지금 메모리창까지 켜져있지만 지금은 레지스터만 확인하면 된다.   
각각의 창은 ```Debug 👉 Show registers, Show memory```를 클릭하거나 단축키 **Ctrl+R**, **Ctrl+M**으로 열고 닫았다 할 수 있다.   
<img src="/assets/images/posts_img/basics/asm/reg-basic/mov1-debug-1.PNG" width="50%">
<img src="/assets/images/posts_img/basics/asm/reg-basic/mov1-debug-1-result.PNG" width="40%">   
화살표가 멈춘 곳은 실행이 완료된 부분이 아니라 해당 코드를 실행할 것이라는 의미이다. 한 줄씩 다음으로 넘어가려면 **Step over**나 **Step into**를 사용하면 된다. 지금은 Step over 방식으로 디버그를 진행시켜보자.
> 💡   
**Step over(단축키 F10)**는 코드에 함수가 있는 경우 해당 함수가 실행되고 난 후의 결과값을 바로 뽑아서 보여주고 다음으로 넘어가는 방식이라면 **Step into(단축키 F11)**는 해당 함수 내의 코드까지 디버그를 해주는 방식이다.

<img src="/assets/images/posts_img/basics/asm/reg-basic/mov1-debug-2.PNG" width="50%">
<img src="/assets/images/posts_img/basics/asm/reg-basic/mov1-debug-2-result.PNG" width="40%">   
F10을 눌러 다음으로 넘겼더니 ```mov eax, 0x1234``` 구문이 실행되면서 우측 레지스터 화면의 rax에 0x1234라는 값이 잘 들어간 걸 확인할 수 있었다.   

위 사진에서 보면 eax는 보이지 않고 rax의 값이 바뀐 걸 알 수 있는데, 내 환경이 64비트라 레지스터 이름은 rax지만 그 rax의 반인 eax부분만 사용하여 값이 들어갔다는 걸로 유추할 수 있다.   

***

### 🌱 최하위 1 Byte와 레지스터간의 데이터 이동
그럼 여기서 레지스터를 풀로 사용하는 게 아닌, 반이나 어쩌면 더 작은 부분만을 사용한다면 어떻게 저장이 되는건지에 대해 알아볼 필요가 있다.   

<img src="/assets/images/posts_img/basics/asm/reg-basic/mov1-debug-3.PNG" width="50%">
<img src="/assets/images/posts_img/basics/asm/reg-basic/mov1-debug-3-result.PNG" width="40%">   
우선 추가적으로 코드를 더 작성한 후에 **Break Point**를 걸었다. 이걸 걸고 디버그 버튼을 반복적으로 누르게되면 해당 포인트에서 멈추게 된다. (아무 포인트도 잡지 않고 디버그 버튼을 반복적으로 누르면 디버그가 바로 종료되어버린다.)   
일단 여기까지 본 결과 각 범용 레지스터엔 값이 잘 들어갔다는 것을 볼 수 있다.   

![Alt text](/assets/images/posts_img/basics/asm/reg-basic/mov1-debug-4-result.PNG)   
하지만 여기서 F10을 눌러 디버그를 한 줄 진행시키게 되면 rax에 **0x00**이라는 값이 들어가야하는데 **0x1200**이라는 값이 들어간 걸 확인할 수 있다.   
al은 가장 최하단 1바이트를 지정하는 이름이기 때문에 이 부분을 0으로 덮어버리게 되는데, ah부분과 그 이상 부분은 영향을 받지 않아서 끝 두 자리만 00으로 변경되는 것이다.   
> 0x12**34** 👉 0x12**00**

![Alt text](/assets/images/posts_img/basics/asm/reg-basic/mov1-debug-5-result.PNG)   
한 번 더 F10을 누르면 rdx에 있는 값이 rax에 잘 들어가는 것을 확인할 수 있다.

***

## 👻 글을 마치며
이론으로만 수도없이 들었던 부분들은 뒤돌면 까먹기 마련인데, 이렇게 어셈블리어를 공부하면서 직접 코드를 짜고 눈으로 보면서 공부하니까 이해도 더 빠르고 그제서야 내 머릿속에 돌아다니던 개념들이 하나 둘 씩 정리되는 기분이다. 기본 개념 공부를 이제와서 하기가 쉽지는 않았는데 하길 잘 한 것 같다 :)

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_assembly/blob/master/register/reg-mov.asm)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   