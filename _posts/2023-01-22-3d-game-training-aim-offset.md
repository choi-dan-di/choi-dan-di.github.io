---
title: "[3D Game Training] #7. 에임 조준하기"
excerpt: "에임 오프셋을 이용해 에임을 조준하는 기능을 추가해보기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, animation, blueprint, unreal, aim offset]

permalink: /3d-game-training/aim-offset/

toc: true
toc_sticky: true

date: 2023-01-22 23:27:09+0900
last_modified_at: 2023-01-22 23:27:11+0900
---

## 👻 에임 조준하기
**에임 오프셋(Aim Offset)**을 이용하여 총 게임에서 빠질 수 없는 에임을 조준하는 기능을 추가해보자. 앞서 만들어 놓았던 애니메이션에 에임 조준 애니메이션을 추가하고 싶을 때 **에임 오프셋**을 이용하면 쉽게 추가할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aim-offset/create-aim-offset.PNG)   

***

### 🌱 에임 오프셋
상세 페이지로 들어가 ``` Axis Settings ``` 설정을 해주면 에임 오프셋을 만들 준비가 완료된다. 가로 축은 Yaw, 세로 축은 Pitch라 지어주고 범위를 지정해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aim-offset/axis-settings.PNG)   

좌측의 ``` Anim Browser ```에서 원하는 애니메이션을 끌어다 축에 놓으면 설정이 완료된다. 각 축마다 기준이 되는 애니메이션을 설정하면 된다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/aim-offset/aim-offset-result.gif)   

***

### 🌱 적용하기
이제 **애니메이션 블루프린트**로 넘어와서 방금 만든 에임 오프셋을 적용시켜줘야하는데, Yaw 값 같은 경우 이전에 만들어 놓았던 ``` RootYawOffset ```을 이용하면 구할 수 있다. 해당 값의 반대 방향으로 캐릭터가 돌아가기 때문에 **-1을 곱해**준다. Pitch는 마땅한 값이 없기 때문에 변수를 새로 만들고 세팅해준 다음 이용할 수 있다. ``` Get Base Aim Rotation ``` 노드를 이용하여 Pitch 값을 구해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aim-offset/set-pitch.PNG)   

에임 조준 애니메이션은 상체를 움직이므로 ``` AnimGraph ```의 상체 부분에 해당하는 ``` Cached Fire ``` 쪽을 수정해주면 된다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/aim-offset/anim-graph.PNG)   

``` Aim Offset ``` 노드를 이용하여 방금 만든 에임 오프셋을 가져온 뒤 해당하는 값을 연결시켜주면 완성된다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/aim-offset/aim-offset-result2.gif)   
<span style="font-size: 0.7rem; color: gray;">에임을 위로 놔둔 뒤 멈춰서 확인해보면 에임 오프셋이 적용된 것을 확인할 수 있다.</span>

> 💡 **Additive Animation**   
**에임 오프셋**의 경우 블렌드 되는 일반 애니메이션이 아닌 **추가 가능한 애니메이션(Additive Animation)**에 속한다. 두 개의 일반 애니메이션 간 블렌드의 경우 말 그대로 두 개의 애니메이션을 섞어 만들지만 애디티브 애니메이션과 블렌드 시엔 베이스 애니메이션과의 차이를 계산하여 만들어진다. 그 자체로는 쓸모없으나 다른 애니메이션에 더해지면 제대로 작동하게 되는 애니메이션인 것이다.   
예를 들어, **재장전 애니메이션(Additive)**를 만들고, 다른 애니메이션(대기, 전진 등)에 더하게 되면 하나의 애니메이션으로 여러 상태에서 부담 없이 사용할 수 있게 된다. 하지만 일반 애니메이션으로 만들고 블렌드하는 경우 애니메이션 간의 보간이 자연스럽게 이루어지지 않아 어렵고 무거운 점이 없지 않아 있다.   
[👉 공식 문서 참고 👈](https://docs.unrealengine.com/5.0/en-US/aim-offset-in-unreal-engine/)

***

## 👻 글을 마치며
이번 시간에는 에임 오프셋에 대해 알아보고 직접 만들어보았다. 애니메이션만 있으면 적용하는 건 크게 어렵지 않았다. 하지만 에임 오프셋이 애디티브 애니메이션이고, 어떠한 기능인지에 대해선 공식 문서를 찾아봐도 한 번에 이해하기 어려웠다. 보간 차이, 적용 차이인 것 같은데 제대로 잘 이해한 건지 모르겠다. 구글링 스킬을 레벨업 할 때가 됐나보다. 😂😂😂

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/AXLS)_