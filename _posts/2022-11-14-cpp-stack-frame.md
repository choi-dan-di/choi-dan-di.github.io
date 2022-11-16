---
title: "[C++] 스택 프레임"
excerpt: "스택 프레임에 대해 알아보기"

categories:
  - C/C++
tags:
  - [C/C++, C++, Visual Studio, function, stack, frame, stack frame]

permalink: /cpp/stack-frame/

toc: true
toc_sticky: true

date: 2022-11-14 17:14:04+0900
last_modified_at: 2022-11-14 17:14:06+0900
---

## 👻 디버깅 옵션
디버깅 할 때 함수가 있으면 살짝 헷갈리는 부분이 있다. 잠깐 정리하고 다음으로 넘어가자.   

> - **F5** : 디버깅 시작
- **F10** : 프로시저 단위 진행
- **F11** : 한 단계씩 진행

여기서 프로시저는 함수를 의미한다. 브레이크 포인트를 잡은 후에 디버깅을 실행하고, ``` F10 ```을 눌러 디버깅을 진행하면 함수를 만났을 때 결과값만 보여진다. 함수 내부의 진행 상황을 알고 싶다면 ``` F11 ```을 눌러 디버깅을 진행하면 된다. 그러면 함수를 만났을 때 함수 내부로 들어가 한 줄씩 실행되는 과정을 알 수 있다.

***

## 👻 스택 프레임(Stack Frame)
함수를 호출했을 때 어떤 식으로 데이터가 움직이는지 알아보려면 스택 프레임에 관한 개념을 알아야한다. **스택 프레임(Stack Frame)**이란, **스택 영역에 함수를 구분하기 위해 생성되는 공간**으로써 **매개변수, 반환 주소값, 지역변수**등을 포함하고 있으며 함수 호출 시 생성되고 함수가 종료되면서 소멸된다.

메모리 안에 코드 영역, 데이터 영역, 힙 영역, **스택 영역**이 있고 일시적으로 사용되는 공간이다. 함수들이 사용하는 메모장 정도로 생각하면 된다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/img_c_stackframe_01.png)   

스택은 **높은 주소에서 낮은 주소 순으로 영역이 할당**되며 **후입선출(LIFO : Last In First Out) 방식**으로 데이터 이동이 진행된다.

***

### 🌱 눈으로 확인해보기
이전에 만들어뒀던 코드를 디버깅 해보면서 스택에서 함수가 어떻게 저장되는지 직접 확인해보자.

우선 코드는 이렇다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/debug.PNG)   

디버깅 후 어셈블리어를 살펴보자.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/asm.PNG)   

위의 코드를 보면 ``` push ```가 총 두 번 쓰인 걸 알 수 있다. push는 스택에 데이터를 넣는 명령어이고 a와 b(매개변수)가 스택에 올려졌다는 것을 알 수 있다. 방향은 오른쪽부터 왼쪽 순이다.

> 64비트 환경은 레지스터를 최대한 활용하기 때문에 어셈블리코드가 다를 수 있다.

``` Ctrl + Alt + G ```를 눌러 **레지스터 창**을 열어보자.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/register.PNG)   

그 중 **ESP**의 주소값을 복사하여 메모리 주소값에 입력해보면 스택 프레임에 올려져있는 값을 알 수 있다. ESP는 스택에서 현재 커서가 위치한 곳을 의미한다. 우클릭을 눌러 보는 방식을 변경할 수 있다.

***

#### 🪐 매개변수

그다음 ``` F11 ```을 두번 눌러 push까지 실행해보자.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/memory.PNG)   

ESP가 가리키는 바로 위쪽 메모리에 b 변수 값인 5가 저장된 것을 볼 수 있다. 매개변수가 저장된 것이고, 스택 프레임는 높은 주소에서 낮은 주소로 할당되기 때문에 이전 주소값에 저장이 됐다는 것을 알 수 있다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/memory2.PNG)   

``` ESP = 0x00AFF744 ```이고, 한 번의 ``` push eax ``` 명령어가 더 실행 후 a 변수의 값인 3이 위쪽 메모리에 저장되었다.

이렇게 두 개의 매개변수가 스택 프레임에 저장되었다.

***

#### 🪐 반환 주소값
이제 다음 명령어는 ``` call MultiplyBy ```이다. 함수를 호출하는 부분인데, 함수 호출이 끝난 후 넘어가야 할 명령어의 주소값이 스택 프레임에 저장이되는지 확인해보자. 똑같이 ``` F11 ```을 눌러 한 줄을 실행시켜주자. 예상대로라면 매개변수 a의 값이 들어간 주소 이전 주소에 반환 주소값인 ``` 007B2690 ```이 들어갈 것이다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/memory3.PNG)   

값이 잘 들어갔고, 디버깅 포인트는 함수로 점프하라는 명령어로 이동한 것을 알 수 있다. 한 번 더 ``` F11 ```을 누르면 함수 내부로 들어오게 된다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/memory4.PNG)   

함수 내부에서는 ESP를 EBP로 복사하게 된 후 다음 명령어를 진행하게된다. **EBP**는 스택의 상대주소 계산용으로 사용된다. 이 부분이 스택 프레임을 관리하는 부분이라고 보면 된다.

> **EBP와 ESP**   
ESP는 계속 변경되는 주소기 때문에 이것을 기준으로 상대 주소를 구하기엔 어려움이 있다. 그래서 EBP에 값(첫 출발점 ESP값)을 넣어두면 고정이 되기 때문에 상대적으로 주소값 계산이 쉬워진다.

***

#### 🪐 지역변수
지역변수는 함수 내부에서 새로 생성된 변수를 뜻한다. 해당 변수의 주소값도 스택 프레임에 저장되는데 내가 살펴봤던 코드엔 지역변수를 생성해두지 않아서 코드를 추가하고 다시 디버깅을 진행했다. 디버깅을 진행할 때마다 스택 프레임의 주소가 바뀌지만 저장 위치는 동일하니 참고하자.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/asm2.PNG)   

그렇게 ``` int c = a * b; ```를 추가해준 다음, c의 값이 어디 저장되는지 확인하기위해 해당 부분에 브레이크 포인트를 잡고 디버깅을 진행해보았다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/memory5.PNG)   

매개변수 5와 3이 저장되고, 반환 주소값, EBP, 그 다음 한 주소 건너뛰고 다음 주소값에 c의 값(3 * 5 = 15)이 저장되었다는 것을 알 수 있다. 건너뛰는건 컴파일러 마음이지만 제일 낮은 주소값을 가진다는 것은 동일하다.

***

#### 🪐 스택 프레임 사용 종료

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/asm3.PNG)   

이렇게 스택을 다 사용하고나면 **반환 주소값**을 통해 다시 빠져나오게 되며 할당받았던 스택 프레임 내의 데이터는 pop을 사용해 지워지게된다.

![Alt Text](/assets/images/posts_img/basics/cpp/function/stack-frame/asm4.PNG)   

반환 주소값으로 **ret(return)** 하는걸로 스택 프레임 사용이 종료되는 것까지 확인할 수 있었다.

***

## 👻 글을 마치며
이번 시간에는 스택 프레임에 대해 알아보고 직접 디버깅을 통해 어떤 식으로 데이터가 움직이는지 알아보았다. 어셈블리로 살펴볼 때는 조금 복잡했지만 눈에 직접 보면서 여러번 반복하니 정리가 잘 되는 것 같았다. 메모리 활용에 굉장히 유용한 것 같다는 생각이 들었다. 어떻게 만들었는지 그저 신기할 따름이다..👀

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_cpp/tree/main/function/stack-frame)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/bje8)_   