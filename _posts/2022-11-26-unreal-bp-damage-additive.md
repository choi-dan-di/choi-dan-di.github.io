---
title: "[Unreal Engine] BP - 연습 문제 : 데미지 합산기"
excerpt: "블루프린트에서 맵을 이용하여 데미지를 합산하는 기능을 구현해보기"

categories:
  - Unreal Engine
tags:
  - [Unreal Engine, ue, unreal, ue4, ue5, blueprint, bp, data structure, hash table, hash, practice, damage additive]

permalink: /unreal/bp-damage-additive/

toc: true
toc_sticky: true

date: 2022-11-26 00:04:48+0900
last_modified_at: 2022-11-26 00:04:52+0900
---

## 👻 데미지 합산기
RPG에서 보스 몬스터에 준 데미지에 비례하여 보상을 주는 기능이 있다고 가정해보자. 각 유저마다 보스에 입힌 데미지를 합산하여 출력하는 기능을 구현해보자.

***

### 🌱 맵 출력해보기
우선 테스트 겸 ``` String : Integer ``` 타입의 맵을 하나 생성해서 값을 넣고 출력하는 이벤트를 만들어보았다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-damage-additive/map.PNG)   

테스트 데이터는 이렇게 설정해두었다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-damage-additive/print-damage.PNG)   

그런 다음, 키 배열을 뽑아내 ``` For Each Loop ``` 노드로 하나씩 돌려가며 ``` Value ```까지 같이 찾아서 출력해주었다. ``` Keys ``` 노드와 ``` Values ``` 노드는 동시 사용이 불가능한 것 같다. (Exec 선을 반드시 연결해야 동작하는 것 같다.)

***

### 🌱 데미지 합산하기
각각의 유저를 1, 2, 3번을 누르는 이벤트로 대체한다. 번호를 누를 때마다 해당하는 유저의 데미지가 합산되어 맵에 저장되고 출력을 바로바로 해주는 기능을 구현해보았다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-damage-additive/code1.PNG)   

**String** 타입의 ``` Name ``` 변수와 **Integer** 타입의 ``` Damage ``` 변수를 생성해준 후, 번호를 누르면 각 번호에 따라 유저와 데미지가 설정되도록 해주었다.

![Alt Text](/assets/images/posts_img/engines/unreal/blueprint/data-structure/practice/bp-damage-additive/code2.PNG)   

그런다음 맵의 ``` Find ``` 노드를 이용하여 우선 등록된 유저가 있는 지 체크 후 ``` Add ``` 노드를 이용해 값을 추가해주었다.

``` Find ``` 노드는 찾는 값이 없으면 알아서 **0**으로 세팅(정수 기준)해 값을 추가시켜주기 때문에 ``` Branch ``` 노드가 딱히 필요없다.

***

## 👻 글을 마치며
이번 시간에는 데미지를 합산하는 기능을 구현해보며 맵에 대해 조금 더 익혀보았다. find 노드 같은 경우 약간의 생략이 되어있어서 설명을 자세히 읽지 않으면 브랜치 등을 더 사용하게 될 것 같다. 그래도 알고나니 굉장히 간편하고 편리하게 사용할 수 있어서 좋았던 것 같다.

***

_[소스코드 보러가기](https://github.com/choi-dan-di/study_ue/tree/main/UE5/data-structure/practice/BP_DamageAdditive)_

***

_출처_   
_[인프런 Rookies님 강의](https://inf.run/TSqC)_   