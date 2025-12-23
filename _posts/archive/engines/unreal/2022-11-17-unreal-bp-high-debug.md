---
title: "[Unreal Engine] BP - 고급 디버깅"
excerpt: "블루프린터에서 고급 디버깅 하는 방법 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, function, debugging, debug]

permalink: /unreal/bp-high-debug/

toc: true
toc_sticky: true

date: 2022-11-17 01:55:22+0900
last_modified_at: 2022-11-17 01:55:28+0900
---

## 👻 고급 디버깅
이번 시간엔 블루프린트에서 하는 디버깅 중에 더 고급 방법에 대해 알아보자. **C++**을 공부할 때 다중 함수 호출을 하며 **호출 스택**에 대해 알아봤었다. 비슷한 상황을 디버깅하기 위해 3개의 함수를 생성하고 ``` Func C ```를 제외한 각각의 함수가 다른 함수 하나를 호출하는 방식으로 블루프린트를 만들었다.

- BeginPlay   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-high-debug/beginplay.PNG)   

- Func A   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-high-debug/func-a.PNG)   

- Func B   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-high-debug/func-b.PNG)   

- Func C   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-high-debug/func-c.PNG)   

- **실행 결과**   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-high-debug/result.PNG)   

***

### 🌱 호출 스택(Call Stack)
여기서 이제 예를 하나 들어보자. ``` Func B ``` 쪽에 수상한 점이 발견되어 해당 부분에 브레이크 포인트를 잡고 디버깅을 실행하게되면 해당 위치에서 멈추게 되지만 어느 경로로 여기까지 들어왔는지 알지 못한다. 그럴 땐 **호출 스택(Call Stack)**을 보는 것이 좋다.

상단의 ``` Debug 👉 Blueprint Debugger 👉 Call Stack ``` 탭을 활용하면 호출 스택을 확인할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-high-debug/debug.PNG)   

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/function/bp-high-debug/call-stack.PNG)   

호출 스택 창을 열어놓은 상태에서 다른 함수로 들어가면 새로운 스택이 쌓이게 되고, 함수를 빠져나오면 스택이 사라지게된다.

***

## 👻 글을 마치며
이번 시간에는 블루프린트에서 호출 스택을 어떻게 볼 수 있는지에 대해 알아보았다. C++이랑 언리얼 에디터랑 번갈아가면서 공부하니 확실히 공통점이 많이 느껴졌다. 다만 호출 스택이 어디있는 지 알지는 못했는데 이번 시간에 알게 되어서 좋았다. 호출 스택과 귀에 박히도록 들은 F10과 F11의 차이점도 확실하게 내 것으로 습득 된 것 같다. 디버깅은 이제 문제 없이 할 수 있을 것 같다. ☺

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/function/BP_HighDebug)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   