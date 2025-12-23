---
title: "[Unreal Engine] BP - Branch, Sequence, Flip Flop"
excerpt: "조건에 따라 실행 흐름을 제어할 수 있는 노드(Branch, Sequence, Flip Flop)에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, branch, sequence, flip flop]

permalink: /unreal/bp-branch/

toc: true
toc_sticky: true

date: 2022-11-13 20:09:48+0900
last_modified_at: 2022-11-13 20:09:51+0900
---

## 👻 분기문
이번 시간에는 블루프린트에서 사용할 다양한 분기문에 대해 알아보자. 분기문은 조건에 따라 실행 흐름을 제어하는 명령어 중 한 부분으로 ``` C++ ```로 치면 ``` if ```나 ``` switch ```와 의미가 동일하다.

***

### 🌱 Branch
이전에도 한 번 나왔었지만 특정 조건에 따라 참/거짓으로 길을 나눠주는 노드이다. ``` branch ``` 검색도 가능하고, 단축키 ``` B + 좌클릭 ```으로도 생성 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-branch/branch.PNG)   

***

### 🌱 Sequence
코드의 양이 방대해질 때 사용하기 좋은 노드이다. ``` sequence ``` 검색도 가능하고, 단축키 ``` S + 좌클릭 ```으로도 생성 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-branch/sequence.PNG)   

위 같이 노드가 늘어나다보면 복잡해지고 무거워지게 되는데 이러한 코드를 정리해주기 좋다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-branch/sequence2.PNG)   

**Add Pin plus**를 사용하여 **Then** 개수를 늘릴 수 있게되고, ``` Then 0 ```에 연결되어 있는 노드들이 실행된 후에 차례로 ``` Then 1 ```, ``` Then 2 ```, ... 이렇게 실행되는 방식이다.

잠깐 테스트를 해보자면 이렇게 시퀀스를 사용하여 Print Text 노드를 나눠준 뒤, 실행해보자.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-branch/seq-test.PNG)   

결과가 1, 1, 2 순서로 나온 것을 확인할 수 있다. (최근 게 위에 출력되므로 가장 밑이 먼저 출력된 부분이다.)

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-branch/seq-test-result.PNG)   

***

### 🌱 Flip Flop
핑퐁 방식의 흐름에 도움이 되는 노드이다. 마찬가지로 ``` flip flop ```을 검색하여 생성이 가능하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-branch/flip-flop.PNG)   

A를 실행했다가 B를 실행했다가, 다시 A를 실행하는 방식으로 진행된다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-branch/flip-flop-test.PNG)   

해당 블루프린트의 의미는 키보드 1을 누를 때마다 A, B를 번갈아가며 출력해달라는 의미이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-branch/flip-flop-result.PNG)   

1을 누를 때마다 출력값이 바뀌는 것을 확인할 수 있다.

> 💡   
Flip Flop 기능을 Flip Flop 노드 없이 만드는 연습을 해보자. 많은 도움이 된다.   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/bp-branch/made-flip-flop.PNG)   

***

## 👻 글을 마치며
이번 시간에는 분기문에 대해 알아보았다. 브랜치는 이전에 한 번 다룬 적이 있어서 쉽게 알 수 있었고, 시퀀스와 플립 플롭은 처음 들어봤었는데, 시퀀스는 거의 노드 정리에 더 가까운 기능이고 플립 플롭은 on/off 스위치 같은 개념인 것 같다. 내가 알고 있는 것들에 비유하면서 공부하니 이해가 더 잘 되는 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/flow-control/BP_Branch)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   