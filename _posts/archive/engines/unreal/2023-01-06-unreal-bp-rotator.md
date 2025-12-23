---
title: "[Unreal Engine] BP - 회전(Rotator)"
excerpt: "블루프린트에서의 회전에 대해 알아보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, vector, rotator]

permalink: /unreal/bp-rotator/

toc: true
toc_sticky: true

date: 2023-01-06 20:15:57+0900
last_modified_at: 2023-01-06 20:16:01+0900
---

## 👻 로테이터
이번 시간에는 **회전(Rotator)**에 대해 알아보자. 벡터는 방향과 크기를 나타내는 개념이었다면, 로테이터는 말 그대로 액터의 회전을 의미하는 개념이다. 벡터와 동일하게 변수 타입을 지정해주면 로테이션의 값을 저장할 수 있다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-rotator/variable.PNG)   

로테이터의 핀을 분리시켜보면 x, y, z 총 세 개의 값이 보인다. 하지만 내부적으로는 w까지 총 네 개의 값이 존재한다. 세 개의 값으로만 액터의 회전을 관리할 경우에는 **짐벌락(Gimbal Lock)** 현상이 발생하기 때문이다.

> 💡 **짐벌락(Gimbal Lock) 현상이란?**   
**짐벌락(Gimbal Lock) 현상**이란 x, y, z 총 세 개의 축 중에서 **두 개의 축이 겹치는 현상**을 의미한다. 그렇게 되면 회전이 원하는 방향으로 되지 않는 문제가 발생하게 된다. 이러한 현상을 방지하기 위해 내부적으로 w라는 값이 존재한다.

***

### 🌱 로테이터 노드
- **Find Look at Rotation**   
스타트에 위치한 액터가 타겟 액터 방향으로의 회전이 하고 싶을 때 사용되는 노드이다. 

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-rotator/find-look-at-rotation.PNG)   

- **RInterp To**   
회전을 시간에 따라 부드럽게 하도록 만들어주는 노드이다. 애니메이터와 비슷하다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/vector-rotator/bp-rotator/rinterp.PNG)   

> 그 외에도 ``` Break Rotator ```, ``` Make Rotator ``` 등 이전 시간에 알아본 벡터와 동일한 기능을 하는 노드들이 많이 존재한다.

***

## 👻 글을 마치며
이번 시간에는 로테이터에 대해 알아보았다. 위치를 먼저 공부하고 다음으로 회전에 대해 알아보았는데 회전 축이 위치 축과는 달리 90도 방향으로 존재한다는 것을 뒤늦게 알아차렸다. 그래서 이해하는데 약간의 시간이 걸렸다. 구글링 해보니 예전에 고등학생 시절 물리 시간에 배웠던 게 생각나면서 금방 이해할 수 있었다. 모르는 게 아닌 까먹고 있었던 것.. 앞으론 까먹지 않도록 복습을 확실히 해야겠다!

회전축은 엄지 척!!! 👍👍👍

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/vector-rotator/BP_Rotator)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   