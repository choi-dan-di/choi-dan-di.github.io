---
title: "[Unreal Engine] BP - 디버깅(Debugging)"
excerpt: "블루프린트로 디버깅하는 방법 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, debug]

permalink: /unreal/bp-debug/

toc: true
toc_sticky: true

date: 2022-11-13 19:27:15+0900
last_modified_at: 2022-11-13 19:27:18+0900
---

## 👻 디버깅
이번 시간에는 블루프린트에서 디버깅하는 방법을 알아볼 것이다.   

각각 80과 100의 값을 가지는 ``` Hp ```와 ``` MaxHp ```를 만들어 나누어보자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-debug/print.PNG)   

값을 출력하려면 **Print Text** 노드를 이용하여 결과값을 확인하면, 80%가 나와야하지만 ``` Hp = 0% ```로 출력된다. **디버깅**을 사용하면 어디서 잘못된 계산이 됐는지 확인할 수 있다.

***

### 🌱 브레이크 포인트 잡기
원하는 노드를 선택한 후 ``` F9 ```를 누르면 **브레이크 포인트**를 잡거나 풀 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-debug/break-point.PNG)   

브레이크 포인트를 잡은 후 실행시키면 해당 지점에서 코드 진행이 멈추게 된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-debug/break-point2.PNG)   

다음 진행을 알고 싶으면 ``` F10 ```을 누르면 된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-control/bp-debug/break-point3.PNG)   

포인트가 다음으로 진행된 것을 볼 수 있다. 디버깅 상태에서 핀에 마우스 오버를 하면 값을 알 수 있다.

이러한 방식으로 문제를 쉽게 해결할 수 있게 되니 디버깅 습관을 생활화하자!

***

## 👻 글을 마치며
이번 시간에는 블루프린트에서 디버깅을 어떻게 하는지, 또 디버깅을 하면 어떤 식으로 표시를 해주는 지 알게 되었고 단축키가 다른 툴과 비슷해서 금세 외울 수 있었다. 마우스 오버를 하면 값이 다 나오니 참고하면 좋을 것 같다. 개인적인 생각으로, 다른 프로그래밍보다 오류가 난다기보다는 잘못된 값이 더 많이 나오는 것 같은데 디버깅하는 습관을 들이면 조금 더 빨리 문제 포인트를 찾고 수정할 수 있지 않을까하는 생각이 든다.

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   