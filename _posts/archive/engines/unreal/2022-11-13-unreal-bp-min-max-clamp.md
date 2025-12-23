---
title: "[Unreal Engine] BP - 연습 문제 : Min, Max, Clamp"
excerpt: "블루프린트의 분기문을 이용해 연습 문제 풀어보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, branch, sequence, flip flop, practice]

permalink: /unreal/bp-min-max-clamp/

toc: true
toc_sticky: true

date: 2022-11-13 20:57:05+0900
last_modified_at: 2022-11-13 20:57:08+0900
---

## 👻 연습 문제
간단한 전투 게임 알고리즘을 예시로 들어 연습 문제를 풀어보자.

> - 키보드 1을 누를 때마다 Hp가 10씩 깎이고, 현재 체력을 출력해준다. 단, 0을 최소값으로 둔다.
- 키보드 2를 누를 때마다 Hp가 20씩 증가하고, 현재 체력을 출력해준다. 단, MaxHp값을 최대값으로 둔다.

***

### 🌱 Damage 10
이전 시간에 총알 재장전이랑 비슷하다. 좌클릭 대신 1을 누를 때마다 Hp를 10씩 감소시켜주면 되는데, 이번에도 최소값을 설정해야한다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-min-max-clamp/damage-10.PNG)   

이런식으로 간단하게 금방 구현이 가능하다.

***

### 🌱 Heal 20
Damage 10과 별 다르지 않다. 이전엔 최소값을 설정했다면, 이번엔 최대값을 설정하는 그 차이이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-min-max-clamp/heal-20.PNG)   

이전보다 복잡하긴한데 잘 돌아가긴한다.

***

## 👻 Min과 Max
블루프린트에 ``` Min ```, ``` Max ``` 노드가 있다. 입력값 중에 작은 값 혹은 큰 값을 출력해주는 노드이다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-min-max-clamp/min-max.PNG)   

이러한 노드를 잘 활용하면 위에서 만들었던 복잡한 블루프린트가 훨씬 간단해질 수 있다.

- Damage 10 블루프린트 수정   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-min-max-clamp/damage2.PNG)   

- Heal 20 블루프린트 수정   
![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-min-max-clamp/heal2.PNG)   

***

### 🌱 Clamp
입력값이 Min, Max 값을 넘지 않도록 해주는 노드이다. Min과 Max가 합쳐진 노드라고 생각하면 쉽다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/flow-control/practice/bp-min-max-clamp/clamp.PNG)   

``` Clamp ```를 사용하면 위에서 만들었던 Damage와 Heal을 통합시킬 수 있다. RPG 게임을 기준으로 만든다고 했을 때 꼭 합쳐야 되는 건 아니다.

***

## 👻 글을 마치며
이번 시간에는 연습 문제를 통해 데미지, 힐 기능을 익혔고 최소값, 최대값에 대해 공부해보았다. 커트라인을 지정해주는 노드가 있는 건 되게 편리한 것 같다. 내가 고려해야 할 조건의 수가 많아질수록 효율적으로 짜리 어려워지는데 그걸 어느정도 도와주는 기능같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/flow-control/practice/BP_MinMaxClamp)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   