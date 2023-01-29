---
title: "[3D Game Training] #12. 몬스터 피격 판정 설정하기"
excerpt: "몬스터 피격을 판정하는 기능을 구현해보기"

categories:
  - 3D Game Training
tags:
  - [3D Game Training, animation, blueprint, unreal, ai, damaged]

permalink: /3d-game-training/monster-on-damaged/

toc: true
toc_sticky: true

date: 2023-01-27 18:21:24+0900
last_modified_at: 2023-01-27 18:21:28+0900
---

## 👻 피격 판정
플레이어가 슈팅했을 때 몬스터에 맞았는지 확인하는 기능을 구현해볼 것이다. 그 전에 맵과 몬스터의 메쉬를 마켓플레이스에서 찾아 적용해주었다.

몬스터의 피격을 판정하기 위해선 ``` Physics Assets ```을 스켈레탈 메쉬에 연결해줘야한다. 피직스 애셋은 스켈레탈 메쉬에 사용되는 **콜리전(Collision)**과 **물리(Physics)**를 정의하는 데 사용된다. 해당 기능을 통해 헤드샷 등의 판정을 구분할 수 있게 된다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/monster-on-damaged/physics-asset.PNG)   

스켈레탈 메쉬 페이지에서 피직스 애셋을 연결시켜주면 된다. 없으면 새로 생성도 가능하다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/monster-on-damaged/skeleton-cylshadow.PNG)   

피직스 애셋은 스켈레톤 단위로 설정이 가능하다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/monster-on-damaged/shooting.gif)   
<span style="font-size: 0.7rem; color: gray;">몬스터를 피격할 때마다 폭발 이펙트와 사운드가 실행된다.</span>

***

## 👻 몬스터 AI 수정하기
비헤이비어 트리를 이용하여 몬스터의 사거리 내에 타겟(플레이어)이 들어왔을 때 공격 애니메이션을 실행시키도록 수정해보자. 우선 데코레이터의 ``` inverse condition ``` 옵션을 통해 True/False일 때의 실행 분기를 자유롭게 설정할 수 있다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/monster-on-damaged/inverse-condition.PNG)   

디폴트 값은 **체크 해제** 되어있고 **True**일 때 실행된다. ``` inverse condition ```에 체크를 하게되면 데코레이터의 결과가 **False**일 때 실행된다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/monster-on-damaged/inverse-condition2.PNG)   
<span style="font-size: 0.7rem; color: gray;">inverse condition이 거짓으로 설정되어있는 데코레이터</span>

플레이어 때와 마찬가지로 애니메이션만 따로 관리하기 위해 몬스터용 **애니메이션 블루프린트**를 만들어주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/monster-on-damaged/monster-anim.PNG)   

해당 애니메이션 블루프린트를 적용시키면 앞으로 걸어가는 모션이 쭉 적용되다 사거리 내에 타겟이 들어오게 되면 공격 모션을 실행시키게 될 것이다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/monster-on-damaged/attack.PNG)   

또한 몬스터의 **블루프린트 클래스**에 커스텀 이벤트 ``` Attack ```을 추가해주고 애니메이션 전환에 필요한 변수 ``` bAttack ```을 설정해주었다. True로 변경해주어 공격 모션을 실행시킨 후 딜레이 노드를 이용해 0.5초 지연 후에 다시 False로 바꿔주어 공격 모션을 무한 반복하지 않도록 설정해주었다.

![Alt Text](/assets/images/posts_img/projects/3d-game-training/monster-on-damaged/attack2.PNG)   

공격 모션을 지속적으로 진행할 때 플레이어가 이동하게 되면 방향이 따라 바뀌지 않는 부분을 살짝 수정해주었다. ``` Find Look at Rotation ``` 노드를 이용하면 ``` Target ``` 방향으로 회전하게 될 것이다.

- **결과**   
![Alt Text](/assets/images/posts_img/projects/3d-game-training/monster-on-damaged/attack-result.gif)   

***

## 👻 글을 마치며
이로써 3D 게임(이라고 적고 실습이라 읽는)을 만들어보며 언리얼 엔진 에디터의 많은 기능과 블루프린트에 대해 많이 알게 되었다. 이제 기초적인 이론과 블루프린트 사용의 대략적인 방법을 알 수 있게 되었고, 언리얼 엔진 에디터에 수많은 기능들을 언제 어디에서 사용해야 할지 어느정도 공부해보았다. 이제 배웠던 기능들을 개인 프로젝트를 만들어보며 능숙해지도록 연습하고 블루프린트를 C++로 변환하는 공부를 새로 시작해 볼 것이다.

***

_출처_      
_[인프런 Rookies님 강의](https://inf.run/AXLS)_   
_[Physics Asset이란?](https://bbagwang.com/unreal-engine/ue4-%EC%97%90%EC%84%9C%EC%9D%98-physics-asset/)_